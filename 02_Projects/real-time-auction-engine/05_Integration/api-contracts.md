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

#### 1. `POST /auth/login-niro`
Login SSO khusus peserta lelang menggunakan kredensial token dari sistem NIRO. Otomatis membuatkan akun lokal jika pertama kali masuk.

* **Headers:** `Content-Type: application/json`
* **Request Body:**
  ```json
  {
    "niro_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.niro_sample_token..."
  }
  ```
* **Response 200 OK (Pengguna Lama Terdaftar):**
  ```json
  {
    "success": true,
    "data": {
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
* **Response 201 Created (Pengguna Baru Pertama Kali):**
  ```json
  {
    "success": true,
    "data": {
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
* **Response 401 Unauthorized:**
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

#### 2. `POST /auth/login-superadmin`
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
Mengambil daftar snapshot seluruh lot barang bernilai tinggi aktif di pasar.

* **Response 200 OK:**
  ```json
  {
    "success": true,
    "data": [
      {
        "id": "e0b9b3e1-872b-42e7-91a6-f7615cb38421",
        "title": "Cosmograph Chronograph Steel 2023",
        "category": "LUXURY_WATCHES",
        "condition_grade": "MINT",
        "authenticity_guarantee": "Full Set 2023, Box & Papers",
        "image_url": "https://assets.salt-auction.com/chronograph.jpg",
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
        "image_url": "https://assets.salt-auction.com/chronograph.jpg",
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
Mengajukan tawaran lelang dengan memilih tombol inkremental resmi atau tombol buyout instan.

* **Headers:** `Authorization: Bearer <JWT_TOKEN>`, `Content-Type: application/json`
* **Request Body (Contoh 1: Klik Tombol +Rp 10.000.000):**
  ```json
  {
    "increment_amount": 10000000.00,
    "is_buyout_instant": false
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
* **Request Body (Contoh 2: Klik Tombol 'Buyout Langsung'):**
  ```json
  {
    "increment_amount": 0,
    "is_buyout_instant": true
  }
  ```
* **Response 200 OK (Instant Buyout Tercapai — Barang Langsung Laku):**
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

*Parameter Konfigurasi:*
- `min_price` (Wajib): Harga penawaran minimal awal.
- `window_duration_seconds` (Wajib): Jendela waktu lelang (misal 86400 detik untuk 1 hari penuh).
- `max_price` (Opsional): Harga batas atas (Buyout Langsung Laku).
- `anti_sniping_extension_minutes` (Opsional): Durasi perpanjangan (misal: 2 menit, 5 menit) jika terjadi $\ge$ 10 hits penawaran per detik pada 2 menit terakhir lelang.

* **Headers:** `Authorization: Bearer <ADMIN_JWT>`, `Content-Type: application/json`
* **Request Body:**
  ```json
  {
    "title": "Togo Leather Handbag 25 Gold Hardware",
    "category": "DESIGNER_BAGS",
    "condition_grade": "MINT",
    "authenticity_guarantee": "Store Receipt 2024, Original Orange Box, Dustbag, CITES cert",
    "description": "Store fresh condition, highly sought-after collector investment piece.",
    "image_url": "https://assets.salt-auction.com/bag25-gold.jpg",
    "min_price": 250000000.00,
    "allowed_increment_steps": [5000000.00, 10000000.00, 20000000.00],
    "max_price": 400000000.00,
    "window_duration_seconds": 86400,
    "anti_sniping_extension_minutes": 2
  }
  ```
* **Response 201 Created:**
  ```json
  {
    "success": true,
    "data": {
      "id": "c1f76d22-192a-4c28-8742-1928374aef12",
      "title": "Togo Leather Handbag 25 Gold Hardware",
      "category": "DESIGNER_BAGS",
      "model_type": "CAPPED",
      "status": "ACTIVE",
      "current_price": 250000000.00,
      "valuation_ceiling": 400000000.00,
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
      "message": "Harga maksimal harus bernilai lebih besar daripada harga minimal.",
      "details": null
    },
    "timestamp": "2026-10-24T12:30:00.000Z"
  }
  ```

---

### MODUL 7: KONFIGURASI SISTEM DINAMIS

#### 13. `GET /admin/configs`
Mengambil konfigurasi parameter dinamis lelang terkini.

* **Headers:** `Authorization: Bearer <ADMIN_OR_SUPERADMIN_JWT>`
* **Response 200 OK:**
  ```json
  {
    "success": true,
    "data": {
      "anti_sniping_trigger_seconds": 15,
      "anti_sniping_extension_seconds": 30,
      "anti_sniping_max_extension_cap_seconds": 300
    },
    "timestamp": "2026-10-24T12:00:00.000Z"
  }
  ```

---

#### 14. `PUT /admin/configs`
Mengubah konfigurasi dinamis platform secara live tanpa restart server.

* **Headers:** `Authorization: Bearer <ADMIN_OR_SUPERADMIN_JWT>`, `Content-Type: application/json`
* **Request Body:**
  ```json
  {
    "anti_sniping_trigger_seconds": 20,
    "anti_sniping_extension_seconds": 45,
    "anti_sniping_max_extension_cap_seconds": 600
  }
  ```
* **Response 200 OK:**
  ```json
  {
    "success": true,
    "data": {
      "anti_sniping_trigger_seconds": 20,
      "anti_sniping_extension_seconds": 45,
      "anti_sniping_max_extension_cap_seconds": 600,
      "updated_at": "2026-10-24T12:35:00.000Z"
    },
    "timestamp": "2026-10-24T12:35:00.000Z"
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
