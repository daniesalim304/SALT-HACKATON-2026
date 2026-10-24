# Diagram Urutan Interaksi (Sequence Diagrams) — Fase 1 (Core / MVP)

| Metadata | Nilai |
|---|---|
| **Dokumen** | Sequence Diagrams (MVP 1 Baseline) |
| **Proyek** | Real-Time Auction & Bidding Engine |
| **Tipe Dokumen** | Perancangan Arsitektur & Interaksi Sistem (Technical Design) |
| **Versi** | v0.1.0 |
| **Status** | `Draft` (Dalam Perancangan) |
| **Tanggal** | 2026-10-24 |
| **Target SLA** | Latensi Sinkronisasi < 300 ms, Nol Race Condition |

---

## 1. Sequence Diagram: Arsitektur Dual-Login (NIRO SSO vs Superadmin Internal)

### 1.1 Jalur 1: Login with NIRO (Khusus Peserta Lelang / Bidder)
Peserta lelang cukup menekan satu tombol tanpa perlu registrasi atau membuat password baru di sistem lelang.

```mermaid
sequenceDiagram
    autonumber
    actor Bidder as Peserta Lelang (Bidder)
    participant UI as Next.js Web App
    participant NiroAuth as Sistem Autentikasi NIRO (SSO)
    participant API as Golang Auth Service
    participant DB as PostgreSQL Database

    Bidder->>UI: Klik tombol "Masuk dengan NIRO"
    UI->>NiroAuth: Buka dialog / Redirect SSO NIRO
    Bidder->>NiroAuth: Konfirmasi login di akun NIRO
    NiroAuth-->>UI: Kembalikan valid niro_token
    
    UI->>API: POST /api/v1/auth/login-niro {niro_token}
    API->>NiroAuth: Verifikasi keabsahan token & ambil profil user
    NiroAuth-->>API: Profil NIRO {niro_user_id, username, email}
    
    API->>DB: SELECT * FROM users WHERE niro_user_id = $1
    alt Pengguna Baru Pertama Kali Masuk via NIRO
        rect rgb(240, 248, 255)
            Note over API,DB: Otomatis Buat Akun & Inisialisasi Dompet
            API->>DB: INSERT INTO users (auth_provider='NIRO', role='BIDDER', niro_user_id, username, email)
            API->>DB: INSERT INTO wallets (user_id, available_balance=0, reserved_minimum_balance=50000)
            API->>DB: INSERT INTO ledger_entries (type='INITIAL_MINIMUM_RESERVE', amount=50000)
        end
    else Pengguna Sudah Terdaftar
        DB-->>API: Record user ditemukan (Cek role: BIDDER atau ADMIN)
    end

    API->>API: Terbitkan JWT Sesi (Claims: user_id, role, provider='NIRO')
    API-->>UI: 200 OK {token, user_profile, wallet_summary}
    UI-->>Bidder: Masuk ke halaman lelang & tampilkan saldo
```

---

### 1.2 Jalur 2: Login Superadmin via Email & Password dengan Progressive Lockout
Jalur login terproteksi khusus Superadmin dengan proteksi lockout berlipat ganda (*exponential backoff*).

```mermaid
sequenceDiagram
    autonumber
    actor Super as Superadmin Platform
    participant UI as Next.js Web App (Portal Superadmin)
    participant API as Golang Auth Service
    participant Redis as Redis Cache (Lockout Tracker)
    participant DB as PostgreSQL Database

    Super->>UI: Input Email & Password (min. 12 karakter)
    UI->>API: POST /api/v1/auth/login-superadmin {email, password}
    
    API->>Redis: GET lockout:superadmin:{email}
    alt Akun Masih Dalam Masa Lockout
        Redis-->>API: Status: LOCKED {ttl_remaining}
        API-->>UI: 429 Too Many Requests ("Akun terkunci sementara. Coba lagi nanti.")
        UI-->>Super: Tampilkan timer hitung mundur masa lockout
    else Akun Bebas / Tidak Terkunci
        API->>DB: SELECT * FROM users WHERE email = $1 AND role = 'SUPERADMIN'
        DB-->>API: Record Superadmin (password_hash Bcrypt)
        API->>API: Verifikasi Bcrypt Hash Match
        
        alt Kredensial Salah
            API->>Redis: INCR fail_count:superadmin:{email}
            Redis-->>API: Total Gagal (misal: 3, 6, 9)
            opt 3x Salah Pertama
                API->>Redis: SETEX lockout:superadmin:{email} 60 "LEVEL_1" (Kunci 1 Menit)
            end
            opt 6x Salah Kedua
                API->>Redis: SETEX lockout:superadmin:{email} 120 "LEVEL_2" (Kunci 2 Menit - Dobel)
            end
            opt 9x Salah Ketiga
                API->>Redis: SETEX lockout:superadmin:{email} 3600 "LEVEL_3" (Kunci 1 Jam)
            end
            API-->>UI: 401 Unauthorized ("Email atau password salah")
        else Kredensial Benar
            API->>Redis: DEL fail_count:superadmin:{email}
            API->>API: Terbitkan JWT Sesi Master (Claims: role='SUPERADMIN')
            API-->>UI: 200 OK {token, role: 'SUPERADMIN'}
            UI-->>Super: Tampilkan Dashboard Panel Kontrol Superadmin
        end
    end

    opt Promosi Pengguna NIRO Menjadi Admin / Auction Manager
        Super->>UI: Buka daftar user NIRO & pilih salah satu user -> Klik "Jadikan Admin"
        UI->>API: PATCH /api/v1/admin/users/{id}/role {role: 'ADMIN'} [Bearer Superadmin JWT]
        API->>DB: UPDATE users SET role = 'ADMIN' WHERE id = $1 AND auth_provider = 'NIRO'
        DB-->>API: Sukses Terupdate
        API-->>UI: 200 OK ("User NIRO berhasil dipromosikan menjadi ADMIN")
        UI-->>Super: Notifikasi sukses di layar
    end
```

---

## 2. Sequence Diagram: Penawaran Manual Atomik & Anti-Sniping (FEAT-004 + FEAT-007)

Ini adalah jalur interaksi paling kritis (*the hot-path*). Menggunakan **Redis Lua Script** untuk menjamin atomisitas, ketiadaan *race condition*, dan pemenuhan SLA latensi < 300 ms.

```mermaid
sequenceDiagram
    autonumber
    actor Bidder as Peserta (Bidder)
    participant Client as Next.js Client
    participant API as Golang Bid Handler
    participant Redis as Redis (Lua Engine & State)
    participant DB as PostgreSQL
    participant WSHub as WebSocket Broadcast Hub
    actor Observers as Seluruh Peserta Lain

    Bidder->>Client: Klik "Place Bid" ($1.200)
    Client->>API: POST /api/v1/auctions/{id}/bids {amount: 1200} [Bearer JWT]
    
    API->>API: Ekstrak user_id dari JWT & validasi payload dasar
    
    rect rgb(255, 245, 238)
        Note over API,Redis: Redis Lua Script: Serialisasi Atomik di Memori (<5ms)
        API->>Redis: EVALSHA bid_placement.lua (lot_id, user_id, amount=1200, now)
        Redis->>Redis: 1. Cek status lot == ACTIVE & now < end_time
        Redis->>Redis: 2. Cek amount >= current_bid + min_increment
        Redis->>Redis: 3. Cek user_available_balance >= amount
        Redis->>Redis: 4. Lock collateral penawar baru ($1.200) -> Pindah ke Locked Balance
        Note over Redis: Saldo penawar lama TETAP TERTAHAN di akunnya sampai periode lelang selesai
        Redis->>Redis: 5. Update current_price = 1200, highest_bidder = user_id
        
        opt Anti-Sniping Trigger (Sisa waktu <= 15 detik)
            Redis->>Redis: end_time = end_time + 30 detik (Max 5 menit cap)
            Redis->>Redis: Tandai flag: time_extended = true
        end
        
        Redis-->>API: Success {current_price: 1200, end_time, new_highest_bidder, time_extended}
    end

    par Asynchronous DB Sync (PostgreSQL Ledger)
        API->>DB: INSERT INTO bids (lot_id, bidder_id, amount, timestamp)
        API->>DB: UPDATE auctions SET current_price=1200, end_time=...
        API->>DB: UPDATE wallets SET locked_balance=...
    and Real-Time Client Push (<300ms SLA)
        API->>WSHub: Publish Event: BID_ACCEPTED {lot_id, amount: 1200, end_time, time_extended}
        WSHub-->>Client: WS Push: Event BID_ACCEPTED (Update UI Pemimpin)
        WSHub-->>Observers: WS Push: Event PRICE_UPDATED (Update Layar Semua Peserta)
    end

    API-->>Client: 200 OK {status: "BID_CONFIRMED", current_price: 1200}
    Client-->>Bidder: Tampilkan status 'Anda Sedang Memimpin!'
```

---

## 3. Sequence Diagram: Pembelian Instan Model Capped Value (FEAT-006)

Interaksi saat penawaran masuk menyamai atau melampaui batas valuasi (*ceiling*). Hitung mundur lelang langsung dihentikan dan hak aset dikunci.

```mermaid
sequenceDiagram
    autonumber
    actor Bidder as Peserta (Bidder)
    participant API as Golang Bid Handler
    participant Redis as Redis State
    participant DB as PostgreSQL
    participant SettleSvc as Settlement Engine
    participant WSHub as WebSocket Broadcast Hub
    actor Observers as Seluruh Peserta Lain

    Bidder->>API: POST /api/v1/auctions/{id}/bids {amount: 5000} (Mencapai Ceiling $5.000)
    
    API->>Redis: EVALSHA capped_bid.lua (lot_id, user_id, amount=5000)
    Redis->>Redis: Validasi: amount >= valuation_ceiling ($5.000)
    Redis->>Redis: Kunci pemenang & ubah status lot -> CLOSED_AWARDED
    Redis->>Redis: Hentikan timer lelang seketika
    Redis-->>API: Status: BUYOUT_TRIGGERED {winner: user_id, price: 5000}

    par Notifikasi Siaran Instan
        API->>WSHub: Publish Event: AUCTION_BUYOUT_CLOSED {lot_id, winner_id, price: 5000}
        WSHub-->>Observers: WS Push: Lot Ditutup (Terjual via Instant Buyout)
    and Eksekusi Settlement Segera
        API->>SettleSvc: TriggerSettlement(lot_id, winner_id, price=5000)
        SettleSvc->>DB: BEGIN TRANSACTION
        SettleSvc->>DB: UPDATE auctions SET status='AWARDED', winner_id=user_id
        SettleSvc->>DB: UPDATE wallets SET locked_balance = locked_balance - 5000 (Deduct)
        SettleSvc->>DB: INSERT INTO asset_ownership (lot_id, owner_id=user_id)
        SettleSvc->>DB: COMMIT
    end

    API-->>Bidder: 200 OK (Selamat! Anda berhasil membeli aset via Instant Buyout)
```

---

## 4. Sequence Diagram: Penutupan Waktu & Rekonsiliasi Otomatis (FEAT-008)

Interaksi latar belakang saat lot model *Uncapped* habis waktunya (*timer = 0*).

```mermaid
sequenceDiagram
    autonumber
    participant Worker as Golang Expiration Daemon
    participant Redis as Redis State
    participant SettleSvc as Settlement Engine
    participant DB as PostgreSQL
    participant WSHub as WebSocket Broadcast Hub
    actor Winner as Pemenang Lelang
    actor Losers as Peserta Lain yang Kalah

    loop Setiap 1 Detik
        Worker->>Redis: ZRANGEBYSCORE active_auctions -inf now()
        Redis-->>Worker: List lot yang sudah expired [lot_id_101, ...]
    end

    Worker->>Redis: SETNX lock:settle:lot_101 (Distributed Lock, TTL=10s)
    Worker->>SettleSvc: ExecuteSettlement(lot_id_101)
    
    SettleSvc->>Redis: HGETALL auction:lot_101 (Ambil data pemenang & harga final)
    Redis-->>SettleSvc: {highest_bidder: user_A, final_price: 3200}

    rect rgb(245, 255, 245)
        Note over SettleSvc,DB: Rekonsiliasi Finansial & Pemindahan Hak Aset
        SettleSvc->>DB: BEGIN TRANSACTION
        SettleSvc->>DB: UPDATE auctions SET status='SETTLED', winner_id=user_A, final_price=3200
        SettleSvc->>DB: UPDATE wallets SET locked_balance = locked_balance - 3200 WHERE user_id=user_A
        SettleSvc->>DB: INSERT INTO ledger_entries (type='AUCTION_WIN_PAYMENT', amount=3200)
        SettleSvc->>DB: INSERT INTO asset_ownership (lot_id, new_owner_id=user_A)
        SettleSvc->>DB: COMMIT
    end

    SettleSvc->>Redis: DEL auction:lot_101 (Bersihkan state lelang aktif)
    SettleSvc->>WSHub: Publish Event: AUCTION_CONCLUDED {lot_id: 101, winner: user_A, price: 3200}
    
    WSHub-->>Winner: WS Push: Anda memenangkan lot 101! Hak aset telah diterbitkan.
    WSHub-->>Losers: WS Push: Lot 101 selesai. Seluruh saldo agunan Anda telah dilepas.
```
