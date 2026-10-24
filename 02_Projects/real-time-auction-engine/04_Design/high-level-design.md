# High-Level Design (HLD) — Real-Time Luxury Auction Engine

| Metadata | Nilai |
|---|---|
| **Dokumen** | High-Level Design (HLD) & System Architecture |
| **Proyek** | Real-Time Auction & Bidding Engine (Fintech / E-Commerce) |
| **Versi** | v1.1.0 |
| **Status** | `Approved Baseline` |
| **Tanggal** | 2026-10-24 |
| **Target SLA** | Latensi Sinkronisasi < 300 ms, Nol Race Condition, ACID Finansial |

---

## 1. Topologi Arsitektur 4 Lapisan (4-Tier System Topology)

```mermaid
flowchart TD
    subgraph Client_Tier ["1. Client Tier (Presentation)"]
        UI_Bidder["Next.js Web App\n- Katalog Barang Mewah Reaktif\n- Tombol Bid Inkremental\n- Countdown Timer Sinkron Server"]
        UI_Admin["Next.js Admin Console\n- Manajemen Lot & Jendela Waktu\n- Panel Promosi Superadmin\n- Live Monitoring Telemetri"]
    end

    subgraph Auth_Tier ["External Identity"]
        NIRO["Sistem Autentikasi NIRO SSO\n(Identity Provider Peserta)"]
    end

    subgraph App_Tier ["2. Application Tier (Golang Clean Architecture)"]
        Gateway["Reverse Proxy & Rate Limiter\n(Token Auth Middleware, CORS, TLS)"]
        
        subgraph Go_Core ["Golang Monolith / Modular Services"]
            AuthSvc["Auth & RBAC Service\n- Validasi NIRO SSO\n- Bcrypt Internal Superadmin\n- Promosi Role User"]
            BidEngine["Bid Placement Engine\n- Validasi Inkremental Tombol\n- Eksekusi Redis Lua Hot-Path\n- Evaluasi Capped Buyout"]
            CatalogSvc["Catalog & Auction Service\n- Kurasi Barang Mewah\n- Pengaturan Jendela Waktu"]
            ProxyWorker["Proxy Bidding Worker\n- Evaluasi Step Increment Otomatis\n- Resolusi Limit Peserta"]
            SettleDaemon["Settlement Daemon\n- Deteksi Expiry via Redis ZSET\n- Distributed Lock & ACID Ledger"]
            WSHub["WebSocket Broadcast Hub\n- Multiplexing Channel per Lot\n- Push Latensi Rendah < 300 ms"]
        end
    end

    subgraph Memory_Tier ["3. In-Memory Hot-Path Tier (Redis 7)"]
        LuaScripts["Redis Lua Scripts\n(Serialisasi Mutlak & Atomic Mutex)"]
        RedisState["In-Memory State Store\n- auction:{id} HASH\n- wallet:{id} HASH\n- auctions:expiry_zset ZSET"]
        RedisPubSub["Redis Pub/Sub Bus\n(Distribusi Event Antar-Goroutine)"]
    end

    subgraph DB_Tier ["4. Authoritative Persistence Tier (PostgreSQL 16)"]
        PG[("PostgreSQL 16 Relational Ledger\n- users, roles, auth_provider\n- wallets & immutable ledger_entries\n- luxury auctions & proxy_bids\n- immutable bids history\n- asset_ownership & platform_configs")]
    end

    %% Relasi Client ke App
    UI_Bidder -->|HTTPS REST| Gateway
    UI_Bidder -->|WSS Stream| WSHub
    UI_Admin -->|HTTPS REST| Gateway
    UI_Bidder -.->|SSO Handshake| NIRO
    NIRO -.->|Token Verification| AuthSvc

    %% Internal App Routing
    Gateway --> AuthSvc
    Gateway --> BidEngine
    Gateway --> CatalogSvc

    %% Hot-Path Interaksi
    BidEngine -->|EVALSHA (Atomic Bid & Hold)| LuaScripts
    LuaScripts --> RedisState
    LuaScripts -->|Publish Event| RedisPubSub
    RedisPubSub -->|Subscribe| WSHub
    WSHub -->|Push < 300ms| UI_Bidder
    WSHub -->|Push < 300ms| UI_Admin

    %% Persistensi & Daemon
    BidEngine -.->|Async / Batch Insert| PG
    CatalogSvc -->|CRUD Lot| PG
    AuthSvc -->|User & Role Update| PG
    ProxyWorker <-->|Poll & Trigger| RedisState
    SettleDaemon -->|ZRANGEBYSCORE Expiry| RedisState
    SettleDaemon -->|ACID Final Settlement| PG
```

---

## 2. Pola Arsitektur Backend (Golang Ports & Adapters)

Untuk menjamin prinsip keterujian (*Test-Driven Development / TDD*) dan independensi logika bisnis, kode backend Golang dibagi menjadi 4 layer terpisah:

```text
backend/
├── cmd/api/main.go           # Inisialisasi konfigurasi, koneksi DB, Redis, dan router
├── internal/
│   ├── domain/               # Entitas inti lelang, dompet, aturan inkremental, dan model lelang
│   ├── usecase/              # Logika bisnis (Auth, Bidding, Catalog, Proxy, Settlement)
│   ├── repository/           # Kontrak interface data dan implementasinya (Postgres & Redis)
│   └── transport/            # HTTP Handlers (Gin/Echo/Chi) & WebSocket Event Broadcaster
└── pkg/                      # Modul utilitas independen (JWT, Bcrypt, Logger, Rate Limiter)
```

---

## 3. Justifikasi Keputusan Arsitektur Kritis (ADR Highlights)

| Keputusan Arsitektur | Alternatif yang Dipertimbangkan | Solusi Terpilih & Rationale |
|---|---|---|
| **Penanganan Konkurensi Bidding** | PostgreSQL Row Lock (`SELECT FOR UPDATE`) | **Redis Lua Scripting.** Menjalankan validasi saldo, penguncian harga, unfreeze saldo lama, dan anti-sniping dalam satu siklus memori (< 5 ms), menjamin SLA < 300 ms bebas *lock contention*. |
| **Mekanisme Agunan Finansial** | Saldo Mengendap Wajib Pasif | **Dynamic Hold on Bid.** Saldo pengguna 100% bebas sampai saat pengguna menekan tombol bid; dana tertahan persis sebesar nilai bid dan langsung cair jika tersalip (*instant unfreeze*). |
| **Metode Penawaran Peserta** | Input Bebas Form Angka | **Discrete Increment Buttons.** Menghilangkan risiko kesalahan ketik (*typo*) dan memastikan kenaikan harga teratur sesuai kelipatan yang ditentukan kurasi lot. |
| **Model Siklus Lelang** | Lelang Durasi Kaku / Tetap | **Dual-Model (Capped vs Time Window).** Fleksibilitas tinggi bagi penjual barang mewah: opsi beli langsung jika mencapai harga puncak, atau bersaing hingga akhir jendela waktu (misal 1 hari). |
| **Arsitektur Autentikasi** | Single Auth Provider | **Dual-Login (NIRO SSO + Internal Superadmin).** Mengintegrasikan SSO eksternal organisasi untuk peserta sekaligus mempertahankan kedaulatan kontrol administratif master platform. |
