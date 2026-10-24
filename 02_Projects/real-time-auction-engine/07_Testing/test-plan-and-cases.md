# Rencana Pengujian TDD & Skenario Kasus Uji (Test Plan & Test Cases)

| Metadata | Nilai |
|---|---|
| **Dokumen** | Quality Assurance & Test-Driven Development Plan |
| **Proyek** | Real-Time Auction & Bidding Engine |
| **Tipe Dokumen** | Rencana Pengujian & Kualitas Perangkat Lunak (QA / Testing) |
| **Versi** | v1.0.0 |
| **Status** | `Draft` (Siap Eksekusi TDD) |
| **Tanggal** | 2026-10-24 |
| **Metodologi** | Test-Driven Development (TDD), Clean Architecture Unit Tests |

---

## 1. Strategi Pengujian (TDD Strategy)

Sesuai dengan ketentuan resmi kompetisi Hackathon 2026 (halaman 7 & 8), repositori wajib mempertahankan status **Green CI/CD Pipeline** dan mematuhi metodologi **Test-Driven Development (TDD)**.

Piramida pengujian dibagi menjadi 3 lapisan:
1. **Unit Testing (Domain & Usecase Logic):** Menguji aturan bisnis lelang murni, perhitungan saldo agunan, dan transisi status lelang dengan mocking pada repository layer.
2. **Integration Testing (Redis Lua & Database Transactions):** Menguji eksekusi atomik skrip Lua di Redis dan transaksi ACID di PostgreSQL nyata (menggunakan testcontainers atau local test DB).
3. **Concurrency & Load Testing:** Simulasi 50–100 goroutine penawaran konkuren untuk memverifikasi ketiadaan *race condition*, kepatuhan latensi < 300 ms, dan ketepatan perpanjangan anti-sniping.

---

## 2. Matriks Kasus Uji (Test Case Matrix)

### Modul 1: Autentikasi, NIRO SSO, dan Dompet Agunan

| Test ID | Skenario Pengujian | Input Data | Hasil yang Diharapkan | Kategori |
|---|---|---|---|---|
| **TC-AUTH-001** | Login pertama kali via NIRO SSO | `niro_token` valid dari user baru | Pengguna dibuat di DB, role = `BIDDER`, saldo dompet otomatis $50.000, JWT terbit. | Unit / Integration |
| **TC-AUTH-002** | Login user NIRO yang sudah terdaftar | `niro_token` valid dari user lama | Profil diambil dari DB, saldo dompet tidak berubah (tetap nilai terakhir), JWT terbit. | Unit |
| **TC-AUTH-003** | Token NIRO palsu / kedaluwarsa | `niro_token` invalid | HTTP `401 Unauthorized`, tidak ada user/dompet baru yang terbuat di DB. | Unit |
| **TC-AUTH-004** | Login Superadmin internal | Username & password benar | HTTP `200 OK`, JWT terbit dengan klaim role `SUPERADMIN`. | Unit |
| **TC-AUTH-005** | Promosi role oleh Superadmin | User ID target, role = `ADMIN` | Record user terupdate menjadi `ADMIN`, user kini dapat mengakses endpoint admin. | Integration |
| **TC-AUTH-006** | Promosi role ditolak jika bukan Superadmin | Request dari user dengan role `BIDDER` | HTTP `403 Forbidden`, role user target tidak berubah. | Unit (Middleware) |

---

### Modul 2: Penawaran Atomik & Saldo Agunan (Hot-Path Engine)

| Test ID | Skenario Pengujian | Input Data | Hasil yang Diharapkan | Kategori |
|---|---|---|---|---|
| **TC-BID-001** | Penawaran manual valid di atas harga terkini | Lot aktif ($1.000, min inc: $100), Bid = $1.200 | Tawaran diterima, harga jadi $1.200, saldo penawar dikunci $1.200, respon < 300 ms. | Integration (Lua) |
| **TC-BID-002** | Penawaran ditolak jika kenaikan di bawah aturan | Current price $1.000, min inc: $100, Bid = $1.050 | HTTP `400 Bad Request` (`ERR_BID_TOO_LOW`), saldo tidak terpotong. | Unit / Lua |
| **TC-BID-003** | Penawaran ditolak jika saldo agunan tidak cukup | Available balance = $500, Bid = $1.000 | HTTP `400 Bad Request` (`ERR_INSUFFICIENT_COLLATERAL`), tawaran ditolak. | Unit / Lua |
| **TC-BID-004** | Otomatis unfreeze saldo penawar lama saat tersalip | User A memimpin $1.000; User B menawar $1.200 | Saldo terkunci User A kembali ke *Available*, saldo User B terkunci $1.200. | Integration (Lua) |
| **TC-BID-005** | Uji Konkurensi Masif (Race Condition Test) | 50 Goroutine mengirim bid $1.200 di milidetik sama | **Hanya 1 bid yang sukses**, 49 lainnya ditolak (`ERR_BID_TOO_LOW`), saldo konsisten. | Concurrency Test |

---

### Modul 3: Dynamic Anti-Sniping & Dual Model Logic

| Test ID | Skenario Pengujian | Input Data | Hasil yang Diharapkan | Kategori |
|---|---|---|---|---|
| **TC-TIME-001** | Penawaran normal di luar zona kritis anti-sniping | Sisa waktu 2 menit, Bid valid masuk | Tawaran diterima, sisa waktu **tidak berubah**. | Unit / Lua |
| **TC-TIME-002** | Anti-sniping aktif pada sisa waktu $\le$ 15 detik | Sisa waktu 8 detik, Bid valid masuk | Tawaran diterima, waktu lelang bertambah **+30 detik** dari jadwal semula. | Integration (Lua) |
| **TC-TIME-003** | Batas maksimal anti-sniping (Max Cap 5 Menit) | Sisa waktu 5 detik setelah diperpanjang 5 menit | Tawaran diterima, tetapi waktu **tidak diperpanjang lagi** (mencapai batas batas cap). | Integration (Lua) |
| **TC-CAP-001** | Pembelian instan model Capped Value | Model Capped (Ceiling $5.000), Bid = $5.000 | Lot langsung berstatus `CLOSED_AWARDED`, timer dimatikan, pemenang disahkan. | Integration (Lua) |
| **TC-CAP-002** | Penawaran di bawah ceiling pada model Capped | Model Capped (Ceiling $5.000), Bid = $3.000 | Diproses sebagai penawaran bersaing normal; lelang tetap aktif berjalan. | Unit / Lua |

---

### Modul 4: Rekonsiliasi Otomatis & Pelepasan Agunan (Settlement)

| Test ID | Skenario Pengujian | Input Data | Hasil yang Diharapkan | Kategori |
|---|---|---|---|---|
| **TC-SETTLE-001**| Waktu lelang habis dengan penawar tertinggi | Lot expired, Highest Bidder = User A ($3.500) | Status lot -> `SETTLED`, saldo terkunci User A dipotong permanen, hak aset terbit. | Integration (Postgres) |
| **TC-SETTLE-002**| Saldo peserta lain dipastikan bebas saat lelang selesai | Lot berakhir, User B & C pernah menawar tapi kalah | Saldo terkunci User B & C bernilai 0, saldo bebas utuh tanpa potongan. | Integration (Postgres) |
| **TC-SETTLE-003**| Distributed Lock mencegah eksekusi rekonsiliasi ganda | 2 instance worker mendeteksi lot expired bersamaan | Hanya 1 worker yang memproses settlement; tidak terjadi double debit saldo. | Concurrency Test |
| **TC-SETTLE-004**| Lelang berakhir tanpa penawar sama sekali | Lot expired, zero bids | Status lot -> `EXPIRED_UNSOLD`, tidak ada mutasi saldo, tidak ada error. | Integration |
