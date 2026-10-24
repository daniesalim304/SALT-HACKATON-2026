# Koleksi Kontrak API Lengkap (API Contract Collection)

| Metadata | Nilai |
|---|---|
| **Dokumen** | Complete API Contract Collection & Response Schemas |
| **Proyek** | Real-Time Auction & Bidding Engine (Fintech / E-Commerce) |
| **Versi** | v1.0.0 |
| **Status** | `Approved Baseline` |
| **Tanggal** | 2026-10-24 |
| **Base URL** | `http://localhost:8080/api/v1` |

---

## 1. Standar Format Respon Global

### Respon Sukses (HTTP 200 / 201)
```json
{
  "success": true,
  "data": {},
  "timestamp": "2026-10-24T12:00:00.000Z"
}
```

### Respon Gagal (HTTP 400 / 401 / 403 / 404 / 409 / 500)
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE_STRING",
    "message": "Pesan deskripsi kesalahan dalam bahasa Indonesia.",
    "details": null
  },
  "timestamp": "2026-10-24T12:00:00.000Z"
}
```

---

## 2. Koleksi Endpoint Lengkap Beserta Seluruh Respon

### MODUL 1: AUTENTIKASI DUAL-LOGIN & RBAC

#### 1. `GET /auth/niro/authorize-url`
Frontend (Next.js) memanggil endpoint ini ketika pengguna mengklik tombol *"Login with NIRO"*. Endpoint ini mengembalikan URL redirect otorisasi SSO NIRO beserta state parameter untuk keamanan CSRF.

* **Headers:** *None*
* **Response 200 OK:**
  ```json
  {
    "success": true,
    "data": {
      "redirect_url": "https://auth.niro.internal/oauth2/authorize?client_id=salt_auction_web&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%3A3000%2Fauth%2Fcallback&state=sec_state_981247",
      "state": "sec_state_981247"
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```

---

#### 2. `POST /auth/login/niro`
Setelah Frontend membuka URL redirect NIRO, pengguna berhasil login di sistem NIRO dan FE mendapatkan token/kode NIRO. Frontend kemudian mengirimkan token NIRO tersebut ke Backend melalui endpoint ini.

**Logika Pemeriksaan Akun di Backend:**
- Backend memvalidasi token ke server NIRO dan mengekstrak identitas (`niro_user_id`, `email`, `username`).
- Backend memeriksa tabel `users` berdasarkan `niro_user_id`:
  - **Jika Belum Terdaftar:** Sistem secara otomatis **mendaftarkan akun baru** (`auth_provider = 'NIRO'`, role default `BIDDER`), membuatkan akun dompet (`available_balance = 0`, `locked_balance = 0`), dan menerbitkan token JWT sesi. Respon mengembalikan status `is_new_user: true` (HTTP 201).
  - **Jika Sudah Terdaftar:** Sistem langsung **meloginkan pengguna**, memuat role terkini (`BIDDER` atau `ADMIN` jika sudah dipromosikan), dan menerbitkan token JWT sesi. Respon mengembalikan status `is_new_user: false` (HTTP 200).

* **Headers:** `Content-Type: application/json`
* **Request Body:**
  ```json
  {
    "niro_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.niro_sample_token..."
  }
  ```
* **Response 200 OK (Pengguna Sudah Terdaftar — Berhasil Loginkan):**
  ```json
  {
    "success": true,
    "data": {
      "is_new_user": false,
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.auction_jwt_token...",
      "user": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "niro_user_id": "NIRO-USR-99812",
        "username": "budi_kolektor",
        "email": "budi@collector.id",
        "auth_provider": "NIRO",
        "role": "BIDDER"
      },
      "wallet": {
        "available_balance": 150000000.00,
        "locked_balance": 0.00,
        "total_balance": 150000000.00
      }
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```
* **Response 201 Created (Pengguna Belum Terdaftar — Berhasil Didaftarkan Otomatis):**
  ```json
  {
    "success": true,
    "data": {
      "is_new_user": true,
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.auction_jwt_token...",
      "user": {
        "id": "7ca12345-5717-4562-b3fc-2c963f66bbbb",
        "niro_user_id": "NIRO-USR-10293",
        "username": "siti_investor",
        "email": "siti@luxury.com",
        "auth_provider": "NIRO",
        "role": "BIDDER"
      },
      "wallet": {
        "available_balance": 0.00,
        "locked_balance": 0.00,
        "total_balance": 0.00
      }
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```
* **Response 401 Unauthorized (Token NIRO Invalid/Expired):**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_INVALID_NIRO_TOKEN",
      "message": "Token otentikasi NIRO tidak valid atau telah kedaluwarsa.",
      "details": null
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```

---

#### 3. `POST /auth/login/email`
Login terisolasi khusus Superadmin platform lelang menggunakan Email dan Password internal (minimal 12 karakter), dilengkapi proteksi penguncian bertingkat (*Progressive Lockout*).

*Aturan Kebijakan Gagal Login (Lockout):*
- 3x salah pertama: Terkunci **1 menit**.
- 3x salah kedua (total 6x): Terkunci **2 menit** (dobel waktu).
- 3x salah ketiga (total 9x): Terkunci **1 jam (60 menit)**.
- Catatan: Fitur *Forgot Password* dan *Kickout* ditiadakan.

* **Headers:** `Content-Type: application/json`
* **Request Body:**
  ```json
  {
    "email": "superadmin@salt-auction.internal",
    "password": "MasterAdminSalt2026!"
  }
  ```
* **Response 200 OK (Login Berhasil):**
  ```json
  {
    "success": true,
    "data": {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.superadmin_jwt...",
      "user": {
        "id": "00000000-0000-0000-0000-000000000001",
        "username": "superadmin",
        "email": "superadmin@salt-auction.internal",
        "auth_provider": "INTERNAL",
        "role": "SUPERADMIN"
      }
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```
* **Response 401 Unauthorized (Kredensial Salah):**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_INVALID_CREDENTIALS",
      "message": "Email atau kata sandi salah.",
      "details": {
        "remaining_attempts_before_lock": 2
      }
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```
* **Response 429 Too Many Requests (Akun Terkunci Sementara - Lockout):**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_ACCOUNT_TEMPORARILY_LOCKED",
      "message": "Terlalu banyak percobaan login gagal berturut-turut. Akun Anda dikunci sementara selama 60 detik. Silakan coba lagi nanti.",
      "details": {
        "retry_after_seconds": 60,
        "lockout_level": 1
      }
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```

---

#### 3. `GET /admin/users`
Mengambil daftar pengguna login NIRO untuk dikelola oleh Superadmin.

* **Headers:** `Authorization: Bearer <SUPERADMIN_JWT>`
* **Query Params:** `?page=1&limit=10&role=BIDDER`
* **Response 200 OK:**
  ```json
  {
    "success": true,
    "data": {
      "users": [
        {
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "niro_user_id": "NIRO-USR-99812",
          "username": "budi_kolektor",
          "email": "budi@collector.id",
          "auth_provider": "NIRO",
          "role": "BIDDER",
          "created_at": "2026-10-24T10:00:00.000Z"
        }
      ],
      "pagination": {
        "page": 1,
        "limit": 10,
        "total_items": 1,
        "total_pages": 1
      }
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```
* **Response 403 Forbidden:**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_FORBIDDEN_ACCESS",
      "message": "Akses ditolak. Endpoint ini hanya dapat diakses oleh Superadmin.",
      "details": null
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```

---

#### 4. `PATCH /admin/users/{id}/role`
Superadmin mempromosikan pengguna login NIRO menjadi **ADMIN** (Auction Manager).

* **Headers:** `Authorization: Bearer <SUPERADMIN_JWT>`, `Content-Type: application/json`
* **Request Body:**
  ```json
  {
    "role": "ADMIN"
  }
  ```
* **Response 200 OK:**
  ```json
  {
    "success": true,
    "data": {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "username": "budi_kolektor",
      "previous_role": "BIDDER",
      "new_role": "ADMIN",
      "updated_at": "2026-10-24T12:05:00.000Z"
    },
    "timestamp": "2026-10-24T12:05:00.000Z"
  }
  ```
* **Response 404 Not Found:**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_USER_NOT_FOUND",
      "message": "Pengguna dengan ID tersebut tidak ditemukan.",
      "details": null
    },
    "timestamp": "2026-10-24T12:05:00.000Z"
  }
  ```

---

### MODUL 2: DOMPET LIKUIDITAS & TOP-UP SALDO

#### 5. `GET /wallet/me`
Mengambil informasi saldo pengguna saat ini.

* **Headers:** `Authorization: Bearer <JWT_TOKEN>`
* **Response 200 OK:**
  ```json
  {
    "success": true,
    "data": {
      "user_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "available_balance": 25000000.00,
      "locked_balance": 75000000.00,
      "total_balance": 100000000.00
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```

---

#### 6. `POST /wallet/topup`
Menambahkan saldo bebas (*Available Balance*) untuk modal bidding (*Role BIDDER Required*).
Sesuai aturan bisnis **US-001**, **hanya Bidder yang dapat melakukan top-up**, dan nominal minimal pengisian adalah **Rp 100.000**.

* **Headers:** `Authorization: Bearer <BIDDER_JWT>`, `Content-Type: application/json`
* **Request Body:**
  ```json
  {
    "amount": 50000000.00
  }
  ```
* **Response 200 OK:**
  ```json
  {
    "success": true,
    "data": {
      "topup_amount": 50000000.00,
      "previous_available_balance": 25000000.00,
      "new_available_balance": 75000000.00,
      "locked_balance": 75000000.00,
      "total_balance": 150000000.00
    },
    "timestamp": "2026-10-24T12:10:00.000Z"
  }
  ```
* **Response 400 Bad Request (Nominal di Bawah Minimal Rp 100.000):**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_MINIMUM_TOPUP_AMOUNT",
      "message": "Minimum top up is IDR 100,000.",
      "details": {
        "minimum_required": 100000.00
      }
    },
    "timestamp": "2026-10-24T12:10:00.000Z"
  }
  ```
* **Response 403 Forbidden (Bukan Bidder):**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_FORBIDDEN_ROLE",
      "message": "Akses ditolak. Fitur dompet dan top-up hanya dapat diakses oleh peran Bidder.",
      "details": null
    },
    "timestamp": "2026-10-24T12:10:00.000Z"
  }
  ```
      "locked_balance": 75000000.00,
      "total_balance": 150000000.00
    },
    "timestamp": "2026-10-24T12:10:00.000Z"
  }
  ```
* **Response 400 Bad Request:**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_INVALID_TOPUP_AMOUNT",
      "message": "Nominal top-up harus berupa angka positif lebih dari nol.",
      "details": null
    },
    "timestamp": "2026-10-24T12:10:00.000Z"
  }
  ```

---

### MODUL 3: KATALOG LELANG BARANG MEWAH

#### 7. `GET /auctions/active`
Mengambil daftar snapshot seluruh lot barang bernilai tinggi yang sedang aktif di pasar, mendukung pagination.
Gambar aset di-return dalam format **Base64 Compressed String** (hasil decode dari binary BLOB di DB).

* **Query Params:** `?page=1&limit=10&category=LUXURY_WATCHES`
* **Response 200 OK:**
  ```json
  {
    "success": true,
    "data": {
      "auctions": [
        {
          "id": "e0b9b3e1-872b-42e7-91a6-f7615cb38421",
          "title": "Cosmograph Chronograph Steel 2023",
          "category": "LUXURY_WATCHES",
          "condition_grade": "MINT",
          "authenticity_guarantee": "Full Set 2023, Box & Papers",
          "image_base64_compressed": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEASABIAAD/2wBD...",
          "model_type": "CAPPED",
          "status": "ACTIVE",
          "starting_price": 350000000.00,
          "current_price": 380000000.00,
          "min_increment": 5000000.00,
          "allowed_increment_steps": [5000000.00, 10000000.00, 25000000.00],
          "valuation_ceiling": 500000000.00,
          "highest_bidder_username": "andi_collector",
          "end_time": "2026-10-25T12:00:00.000Z"
        }
      ],
      "pagination": {
        "page": 1,
        "limit": 10,
        "total_items": 1,
        "total_pages": 1
      }
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```

---

#### 8. `GET /auctions/{id}`
Mengambil detail spesifikasi satu lot barang bernilai tinggi beserta riwayat penawaran terakhir.

* **Response 200 OK:**
  ```json
  {
    "success": true,
    "data": {
      "lot": {
        "id": "e0b9b3e1-872b-42e7-91a6-f7615cb38421",
        "title": "Cosmograph Chronograph Steel 2023",
        "category": "LUXURY_WATCHES",
        "condition_grade": "MINT",
        "authenticity_guarantee": "Full Set 2023, Box & Papers",
        "description": "White dial, pristine condition, unworn.",
        "image_base64_compressed": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEASABIAAD/2wBD...",
        "model_type": "CAPPED",
        "status": "ACTIVE",
        "starting_price": 350000000.00,
        "current_price": 380000000.00,
        "allowed_increment_steps": [5000000.00, 10000000.00, 25000000.00],
        "valuation_ceiling": 500000000.00,
        "highest_bidder_id": "8bb30e92-3a52-47d1-ba76-218f2d5f22e1",
        "highest_bidder_username": "andi_collector",
        "start_time": "2026-10-24T12:00:00.000Z",
        "end_time": "2026-10-25T12:00:00.000Z"
      },
      "recent_bids": [
        {
          "bid_id": "7fa12345-872b-42e7-91a6-f7615cb38421",
          "bidder_username": "andi_collector",
          "amount": 380000000.00,
          "created_at": "2026-10-24T12:15:30.000Z"
        }
      ]
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```
* **Response 404 Not Found:**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_AUCTION_NOT_FOUND",
      "message": "Lot lelang dengan ID tersebut tidak ditemukan.",
      "details": null
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```

---

### MODUL 4: PENAWARAN INKREMENTAL & SALDO TERTAHAN (HOT-PATH)

#### 9. `POST /auctions/{id}/bids`
Mengajukan tawaran lelang dengan memilih tombol inkremental resmi (atau menawar langsung nilai ceiling).
Sistem secara otomatis mengevaluasi:
- Jika `current_price + increment_amount >= valuation_ceiling`, lelang **LANGSUNG LAKU (*Instant Buyout*)** seketika tanpa perlu flag manual.

* **Headers:** `Authorization: Bearer <BIDDER_JWT>`, `Content-Type: application/json`
* **Request Body:**
  ```json
  {
    "increment_amount": 10000000.00
  }
  ```
* **Response 200 OK (Tawaran Sukses & Memimpin):**
  ```json
  {
    "success": true,
    "data": {
      "auction_id": "e0b9b3e1-872b-42e7-91a6-f7615cb38421",
      "new_current_price": 390000000.00,
      "bid_status": "WINNING",
      "wallet_state": {
        "locked_collateral_for_this_bid": 390000000.00,
        "remaining_available_balance": 10000000.00,
        "total_balance": 400000000.00
      },
      "time_extended": false,
      "new_end_time": "2026-10-25T12:00:00.000Z",
      "buyout_triggered": false
    },
    "timestamp": "2026-10-24T12:20:00.000Z"
  }
  ```
* **Response 200 OK (Mencapai/Melebihi Ceiling — Instant Buyout Terpicu Otomatis):**
  ```json
  {
    "success": true,
    "data": {
      "auction_id": "e0b9b3e1-872b-42e7-91a6-f7615cb38421",
      "new_current_price": 500000000.00,
      "bid_status": "WINNING",
      "wallet_state": {
        "locked_collateral_for_this_bid": 500000000.00,
        "remaining_available_balance": 50000000.00,
        "total_balance": 550000000.00
      },
      "time_extended": false,
      "buyout_triggered": true,
      "auction_status": "CLOSED_AWARDED"
    },
    "timestamp": "2026-10-24T12:25:00.000Z"
  }
  ```
* **Response 400 Bad Request (Saldo Kurang):**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_INSUFFICIENT_AVAILABLE_BALANCE",
      "message": "Saldo bebas Anda (Rp 25.000.000) tidak mencukupi untuk tawaran sebesar Rp 390.000.000. Silakan lakukan Top-Up.",
      "details": null
    },
    "timestamp": "2026-10-24T12:20:00.000Z"
  }
  ```
* **Response 400 Bad Request (Lelang Sudah Ditutup):**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_AUCTION_NOT_ACTIVE",
      "message": "Lelang ini telah berakhir atau telah dimenangkan via Buyout.",
      "details": null
    },
    "timestamp": "2026-10-24T12:20:00.000Z"
  }
  ```

---

### MODUL 5: BOT PENAWARAN OTOMATIS (AUTOMATED PROXY BIDDING)

#### 10. `POST /auctions/{id}/proxy-bid`
Mendaftarkan atau memperbarui batas maksimal bot proxy bidding.

* **Headers:** `Authorization: Bearer <JWT_TOKEN>`, `Content-Type: application/json`
* **Request Body:**
  ```json
  {
    "max_limit": 450000000.00
  }
  ```
* **Response 200 OK:**
  ```json
  {
    "success": true,
    "data": {
      "auction_id": "e0b9b3e1-872b-42e7-91a6-f7615cb38421",
      "proxy_id": "b1827461-872b-42e7-91a6-f7615cb38421",
      "max_limit": 450000000.00,
      "is_active": true,
      "current_auction_price": 395000000.00,
      "your_current_bid": 395000000.00,
      "status": "PROXY_ACTIVE_AND_LEADING"
    },
    "timestamp": "2026-10-24T12:26:00.000Z"
  }
  ```
* **Response 400 Bad Request (Limit Lebih Kecil dari Harga Saat Ini):**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_PROXY_LIMIT_TOO_LOW",
      "message": "Batas limit proxy harus lebih besar dari penawaran saat ini (Rp 390.000.000).",
      "details": null
    },
    "timestamp": "2026-10-24T12:26:00.000Z"
  }
  ```

---

#### 11. `DELETE /auctions/{id}/proxy-bid`
Membatalkan / menonaktifkan aturan bot proxy bidding aktif.

* **Headers:** `Authorization: Bearer <JWT_TOKEN>`
* **Response 200 OK:**
  ```json
  {
    "success": true,
    "data": {
      "auction_id": "e0b9b3e1-872b-42e7-91a6-f7615cb38421",
      "is_active": false
    },
    "message": "Aturan proxy bidding berhasil dinonaktifkan.",
    "timestamp": "2026-10-24T12:27:00.000Z"
  }
  ```

---

### MODUL 6: MANAJEMEN LOT & KURASI ADMIN

#### 12. `POST /admin/auctions`
Admin / Auction Manager membuat listing lelang barang bernilai tinggi baru.
- Endpoint ini menerima payload dalam format **`multipart/form-data`** karena menyertakan unggahan binary berkas gambar aset (`image`).
- Gambar binary yang diunggah langsung disimpan di PostgreSQL sebagai **BLOB (`BYTEA`)** dan dikompresi menjadi format Base64 saat dibaca kembali.
- Pengaturan perpanjangan waktu anti-sniping (`anti_sniping_extension_minutes`) dikonfigurasi langsung pada endpoint ini.

* **Headers:** `Authorization: Bearer <ADMIN_JWT>`, `Content-Type: multipart/form-data`
* **Form Data Fields:**
  - `title` (text): `"Togo Leather Handbag 25 Gold Hardware"`
  - `category` (text): `"DESIGNER_BAGS"`
  - `condition_grade` (text): `"MINT"`
  - `authenticity_guarantee` (text): `"Store Receipt 2024, Original Orange Box, Dustbag, CITES cert"`
  - `description` (text): `"Store fresh condition, highly sought-after collector investment piece."`
  - `image` (binary file): `[File binary gambar JPEG/PNG/WEBP]`
  - `min_price` (number): `250000000.00`
  - `allowed_increment_steps` (json string): `"[5000000, 10000000, 20000000]"`
  - `max_price` (number, opsional): `400000000.00`
  - `window_duration_seconds` (number): `86400`
  - `anti_sniping_extension_minutes` (number, opsional): `2`
* **Response 201 Created:**
  ```json
  {
    "success": true,
    "data": {
      "id": "c1f76d22-192a-4c28-8742-1928374aef12",
      "title": "Togo Leather Handbag 25 Gold Hardware",
      "category": "DESIGNER_BAGS",
      "condition_grade": "MINT",
      "authenticity_guarantee": "Store Receipt 2024, Original Orange Box, Dustbag, CITES cert",
      "image_base64_compressed": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEASABIAAD/2wBD...",
      "model_type": "CAPPED",
      "status": "ACTIVE",
      "current_price": 250000000.00,
      "valuation_ceiling": 400000000.00,
      "anti_sniping_extension_minutes": 2,
      "start_time": "2026-10-24T12:30:00.000Z",
      "end_time": "2026-10-25T12:30:00.000Z"
    },
    "timestamp": "2026-10-24T12:30:00.000Z"
  }
  ```
* **Response 400 Bad Request:**
  ```json
  {
    "success": false,
    "error": {
      "code": "ERR_INVALID_AUCTION_PARAMS",
      "message": "Harga maksimal harus bernilai lebih besar daripada harga minimal, dan file gambar wajib dilampirkan.",
      "details": null
    },
    "timestamp": "2026-10-24T12:30:00.000Z"
  }
  ```

---

## 3. Spesifikasi Protokol WebSocket Lengkap

* **URL Koneksi:** `ws://localhost:8080/api/v1/ws/auctions`
* **Handshake Token Query:** `?token=<JWT_TOKEN>` (opsional jika hanya menonton).

### 3.1 Format Frame Masuk (Inbound Frame dari Client ke Server)

```json
{
  "action": "SUBSCRIBE_LOT",
  "lot_id": "e0b9b3e1-872b-42e7-91a6-f7615cb38421"
}
```

---

### 3.2 Format Frame Siaran Lengkap (Outbound Events dari Server ke Client)

#### 1. Event `PRICE_UPDATED` (Penawaran Baru Sah)
```json
{
  "event": "PRICE_UPDATED",
  "data": {
    "lot_id": "e0b9b3e1-872b-42e7-91a6-f7615cb38421",
    "current_price": 390000000.00,
    "highest_bidder_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "highest_bidder_username": "budi_kolektor",
    "increment_added": 10000000.00,
    "total_bids_count": 28
  },
  "timestamp": "2026-10-24T12:20:00.120Z"
}
```

#### 2. Event `TIME_EXTENDED` (Anti-Sniping Aktif)
```json
{
  "event": "TIME_EXTENDED",
  "data": {
    "lot_id": "e0b9b3e1-872b-42e7-91a6-f7615cb38421",
    "extension_seconds": 30,
    "new_end_time": "2026-10-25T12:00:30.000Z",
    "reason": "ANTI_SNIPING_TRIGGERED"
  },
  "timestamp": "2026-10-24T12:20:00.120Z"
}
```

#### 3. Event `AUCTION_BUYOUT_CLOSED` (Harga Maksimal Tersentuh)
```json
{
  "event": "AUCTION_BUYOUT_CLOSED",
  "data": {
    "lot_id": "e0b9b3e1-872b-42e7-91a6-f7615cb38421",
    "winner_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "winner_username": "budi_kolektor",
    "buyout_price": 500000000.00,
    "closed_reason": "VALUATION_CEILING_REACHED"
  },
  "timestamp": "2026-10-24T12:25:00.050Z"
}
```

#### 4. Event `AUCTION_CONCLUDED` (Selesai Waktu & Settle)
```json
{
  "event": "AUCTION_CONCLUDED",
  "data": {
    "lot_id": "c1f76d22-192a-4c28-8742-1928374aef12",
    "status": "SETTLED",
    "winner_id": "8bb30e92-3a52-47d1-ba76-218f2d5f22e1",
    "winner_username": "andi_collector",
    "final_price": 310000000.00,
    "collateral_unfrozen_for_others": true
  },
  "timestamp": "2026-10-25T12:30:00.200Z"
}
```

#### 5. Event `NEW_LOT_PUBLISHED`
```json
{
  "event": "NEW_LOT_PUBLISHED",
  "data": {
    "lot_id": "c1f76d22-192a-4c28-8742-1928374aef12",
    "title": "Hermès Birkin 25 Togo Gold Hardware",
    "brand": "Hermès",
    "category": "DESIGNER_BAGS",
    "starting_price": 250000000.00,
    "model_type": "CAPPED",
    "end_time": "2026-10-25T12:30:00.000Z"
  },
  "timestamp": "2026-10-24T12:30:00.000Z"
}
```
