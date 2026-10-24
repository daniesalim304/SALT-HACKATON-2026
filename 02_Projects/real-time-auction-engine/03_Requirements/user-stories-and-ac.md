# User Stories & Acceptance Criteria (Updated Baseline)

| Metadata | Nilai |
|---|---|
| **Dokumen** | Updated User Stories & Acceptance Criteria (MVP 1 Baseline) |
| **Proyek** | Real-Time Auction & Bidding Engine (Fintech / E-Commerce) |
| **Versi** | v2.0.0 (Updated Baseline) |
| **Status** | `Approved Baseline` |
| **Tanggal** | 2026-10-24 |
| **Bahasa** | English & Bahasa Indonesia |

---

## Ringkasan Perubahan Kunci (Change Summary v2.0.0):
1. **Multi-Role Authentication (US-AUTH):** 
   - Login via **NIRO SSO** untuk Bidder & Auction Manager; Login via **Email & Password** khusus Superadmin (min. 12 karakter).
   - Fitur *Forgot Password* dan *Kickout* resmi **dihapus (takeout)**.
   - Kebijakan **Progressive Lockout** untuk Superadmin: Salah 3x pertama $\rightarrow$ tunggu 1 menit; salah 3x kedua $\rightarrow$ tunggu 2 menit; salah 3x ketiga $\rightarrow$ tunggu 1 jam.
   - Superadmin memiliki wewenang untuk **Assign / Promote** user NIRO menjadi Auction Manager (AM).
2. **E-Wallet & Top-Up (US-001):**
   - **Hanya Bidder** yang memiliki dompet dan berhak top-up. AM dan Superadmin tidak memiliki akses top-up.
   - Minimal nominal top-up adalah **Rp 100.000**.
3. **Mekanisme Saldo Tertahan (US-002):**
   - Tidak ada saldo minimum mengendap pasif.
   - Saldo baru berstatus **TERTAHAN (Locked)** tepat saat bidding, dan **TETAP TERTAHAN** sepanjang periode lelang sampai lelang berakhir/final.
4. **Bidding & Kategori Barang (US-003):**
   - Barang diklasifikasikan murni berdasarkan **Kategori Barang Bernilai Tinggi** (tanpa field Brand).
   - Penawaran menggunakan tombol inkremental resmi (bukan input teks bebas). Aksi menawar khusus Bidder.
5. **Anti-Sniping Berbasis Kecepatan (US-004):**
   - Aktif pada **2 menit terakhir** lelang; jika terjadi **$\ge$ 10 hits/detik**, waktu diperpanjang otomatis.
   - Besaran menit perpanjangan dikonfigurasi fleksibel oleh Auction Manager saat membuat lot.
6. **Live Bid Monitoring Stream (US-008):**
   - Dapat diakses oleh **ketiga peran (Bidder, AM, Superadmin)**.

---

## Koleksi User Story & Acceptance Criteria Lengkap (JSON Format)

```json
[
  {
    "id": "US-AUTH",
    "title": "US-AUTH - Multi-Role Authentication with Progressive Lockout",
    "description": "As a system user (Bidder, Auction Manager, or Super Admin), I want to log in using the authentication method designated for my role (NIRO SSO for Bidder & Auction Manager, Email & Password for Super Admin), so that I can securely access my dedicated workspace and the system prevents brute force attacks through progressive lockout.",
    "preconditions": [
      "The user is on the unified Sign in page",
      "Bidder and Auction Manager have an active NIRO SSO account",
      "Super Admin has an active account with an Email and Password (minimum 12 characters)",
      "Forgot Password and Automatic Session Kickout features are removed from the system"
    ],
    "business_flow": [
      "The user navigates to the Sign in page",
      "[Bidder / Auction Manager] The user clicks the 'Login with NIRO' button",
      "[Bidder / Auction Manager] The system redirects to NIRO SSO, validates credentials and 2-step verification, and redirects to the appropriate workspace (Bidder Workspace or Auction Manager Console)",
      "[Non-Super Admin via Email/Password] If a Bidder or Auction Manager attempts to log in using the Email/Password form, the system rejects the request and instructs the user to use 'Login with NIRO'",
      "[Super Admin] The user inputs Email and Password (min. 12 characters) and clicks 'Sign in'",
      "[Super Admin Lockout Evaluation] If there are 3 consecutive failed login attempts, the system locks the account for 1 minute. If 3 more consecutive failed attempts occur, lock for 2 minutes (double time). If 3 more consecutive failed attempts occur, lock for 1 hour (60 minutes)",
      "[Success] The system creates a JWT session and redirects the user to their designated workspace"
    ],
    "business_rules": {
      "application_scope": "Authentication & RBAC",
      "user_role": "Bidder, Auction Manager, Super Admin",
      "platform": "Web"
    },
    "acceptance_criteria": [
      {
        "id": "AC-AUTH-001",
        "given": "A Bidder user is on the Sign in page",
        "when": "The user clicks 'Login with NIRO' and successfully authenticates",
        "then": "The system issues a JWT session with role 'BIDDER' and redirects the user to the Bidder Workspace"
      },
      {
        "id": "AC-AUTH-002",
        "given": "An Auction Manager user is on the Sign in page",
        "when": "The user clicks 'Login with NIRO' and successfully authenticates",
        "then": "The system recognizes their promoted 'ADMIN' role and redirects the user to the Auction Manager Workspace (not the Bidder workspace)"
      },
      {
        "id": "AC-AUTH-003",
        "given": "A Bidder or Auction Manager user is on the Sign in page",
        "when": "The user attempts to submit the Email & Password form",
        "then": "The system rejects the login attempt, does not create a session, and displays: 'Please use Login with NIRO for your account type'"
      },
      {
        "id": "AC-AUTH-004",
        "given": "A Super Admin user is on the Sign in page",
        "when": "The user inputs a valid Email and Password (min. 12 characters) and submits",
        "then": "The system issues a JWT session with role 'SUPERADMIN', resets any failed login counters, and redirects to the Super Admin Portal"
      },
      {
        "id": "AC-AUTH-005",
        "given": "A Super Admin user is on the Sign in page",
        "when": "The user enters an incorrect password 3 consecutive times",
        "then": "The system locks the Super Admin login for 1 minute (60 seconds) and displays HTTP 429 with retry-after information"
      },
      {
        "id": "AC-AUTH-006",
        "given": "A Super Admin login has already been locked for 1 minute once",
        "when": "The user fails another 3 consecutive login attempts immediately after the first lock expires",
        "then": "The system doubles the lockout duration to 2 minutes (120 seconds)"
      },
      {
        "id": "AC-AUTH-007",
        "given": "A Super Admin login has already been locked twice (1 minute and 2 minutes)",
        "when": "The user fails another 3 consecutive login attempts",
        "then": "The system escalates the lockout duration to 1 hour (3600 seconds)"
      },
      {
        "id": "AC-AUTH-008",
        "given": "A Super Admin is logged in to the Super Admin Portal",
        "when": "The Super Admin selects an existing NIRO user and assigns the role 'ADMIN' (Auction Manager)",
        "then": "The user's role is updated in the database to 'ADMIN', and the user's active session/token is immediately revoked (auto-logged out). The user must log in again to receive an updated JWT token with the new role."
      }
    ]
  },
  {
    "id": "US-001",
    "title": "US-001 - Top Up E-Wallet Balance (Bidder Exclusive)",
    "description": "As a Bidder, I want to top up my e-wallet balance with a minimum amount of IDR 100,000, so that I have sufficient available balance to participate in and place bids on luxury auctions.",
    "preconditions": [
      "User is logged in with role 'BIDDER'",
      "Auction Managers and Super Admins are prohibited from accessing wallet top-up"
    ],
    "business_flow": [
      "Bidder navigates to the E-Wallet menu",
      "Bidder enters a top-up amount >= IDR 100,000",
      "System validates role == 'BIDDER' and amount >= IDR 100,000",
      "System updates available balance and logs an immutable ledger entry"
    ],
    "business_rules": {
      "application_scope": "E-Wallet - Top Up",
      "user_role": "Bidder Only",
      "platform": "Web"
    },
    "acceptance_criteria": [
      {
        "id": "AC-001-001",
        "given": "A logged-in Bidder is on the E-Wallet page",
        "when": "Bidder submits a top-up amount >= IDR 100,000",
        "then": "The available balance increases by the exact amount, total balance updates, and an entry of type 'WALLET_TOPUP' is logged"
      },
      {
        "id": "AC-001-002",
        "given": "A logged-in Bidder is on the E-Wallet page",
        "when": "Bidder submits a top-up amount < IDR 100,000",
        "then": "System rejects the request with HTTP 400 and error message: 'Minimum top up is IDR 100,000'"
      },
      {
        "id": "AC-001-003",
        "given": "A logged-in Auction Manager or Super Admin",
        "when": "The user attempts to call POST /api/v1/wallet/topup",
        "then": "System rejects with HTTP 403 Forbidden and error message: 'Wallet top-up is exclusive to Bidder accounts'"
      }
    ]
  },
  {
    "id": "US-002",
    "title": "US-002 - Dynamic Collateral Hold During Bidding",
    "description": "As a Bidder, I want my available balance to be validated and held as collateral upon submitting a bid, and to remain held throughout the entire auction period until settlement, so that all bids are backed by real funds.",
    "preconditions": [
      "Bidder is logged in",
      "Available balance = Total balance minus total currently locked active bids",
      "No passive minimum balance is required"
    ],
    "business_flow": [
      "Bidder opens an active auction item",
      "System compares available balance against the target bid amount",
      "If available balance >= target bid, the increment button is enabled",
      "Upon clicking the increment button, the bid amount is instantly moved from available to locked balance",
      "If another bidder outbids this bid, the locked balance REMAINS HELD until the auction concludes",
      "When the auction concludes, winning funds are debited and non-winning held funds are released 100% back to available balance"
    ],
    "business_rules": {
      "application_scope": "Bidding - Collateral Hold",
      "user_role": "Bidder",
      "platform": "Web"
    },
    "acceptance_criteria": [
      {
        "id": "AC-002-001",
        "given": "Bidder has available balance of IDR 100,000 and locked balance of IDR 0",
        "when": "Bidder places a bid of IDR 75,000",
        "then": "Bid is accepted, IDR 75,000 is moved to locked balance, and available balance becomes IDR 25,000"
      },
      {
        "id": "AC-002-002",
        "given": "Bidder has an active held bid of IDR 75,000",
        "when": "Another participant submits a higher bid of IDR 85,000 (outbid event)",
        "then": "The initial bidder's IDR 75,000 remains in locked balance and is NOT immediately released during the ongoing auction period"
      },
      {
        "id": "AC-002-003",
        "given": "Bidder has available balance of IDR 25,000",
        "when": "Bidder attempts to click a bid option requiring IDR 50,000",
        "then": "The button is disabled with an explanatory message: 'Insufficient available balance. Please top up'"
      },
      {
        "id": "AC-002-004",
        "given": "The auction concludes and the Bidder did not win",
        "when": "The settlement daemon processes finality",
        "then": "The Bidder's held IDR 75,000 is released 100% back to available balance"
      }
    ]
  },
  {
    "id": "US-003",
    "title": "US-003 - Place Incremental Bid on an Uncapped Window Auction",
    "description": "As a Bidder, I want to place manual bids using pre-configured increment buttons on high-value asset categories without typing free text, competing within a time window defined by the Auction Manager (e.g. 1 day).",
    "preconditions": [
      "Bidder is logged in and has sufficient available balance",
      "Auction is of Uncapped type, categorized under high-value personal goods, and within its active time window"
    ],
    "business_flow": [
      "Bidder selects an auction item",
      "System displays asset category, current leading price, countdown timer, and increment buttons (e.g. +IDR 10k, +IDR 50k)",
      "Bidder clicks an increment button",
      "Redis Lua script processes the atomic bid in < 5 ms",
      "WebSocket broadcasts the updated price to all connected participants in < 300 ms"
    ],
    "business_rules": {
      "application_scope": "Bidding - Incremental Uncapped",
      "user_role": "Bidder",
      "platform": "Web"
    },
    "acceptance_criteria": [
      {
        "id": "AC-003-001",
        "given": "Current price is IDR 350,000,000 and allowed increments are [+IDR 5m, +IDR 10m]",
        "when": "Bidder clicks '+IDR 10m'",
        "then": "New price becomes IDR 360,000,000, bidder becomes highest bidder, and price updates for all observers in < 300 ms"
      },
      {
        "id": "AC-003-002",
        "given": "Two Bidders click the increment button simultaneously",
        "when": "System processes both requests via Redis Lua atomic serialization",
        "then": "The first arriving bid is accepted, the second is evaluated against the newly incremented price with zero race condition"
      },
      {
        "id": "AC-003-003",
        "given": "An Auction Manager or Super Admin user",
        "when": "Viewing the auction detail page",
        "then": "The bid action buttons are hidden or disabled to prevent conflict of interest"
      }
    ]
  },
  {
    "id": "US-004",
    "title": "US-004 - Velocity-Based Anti-Sniping Dynamic Timer Extension",
    "description": "As a Bidder and Auction Manager, I want the auction timer to automatically extend if high bidding velocity (>= 10 hits/second) is detected in the final 2 minutes, using the extension duration configured for that lot.",
    "preconditions": [
      "Auction is active and has configured anti_sniping_extension_minutes > 0",
      "Remaining time is <= 2 minutes (120 seconds)"
    ],
    "business_flow": [
      "A bid arrives while remaining time <= 120 seconds",
      "System increments the 1-second velocity counter in Redis",
      "If the counter reaches >= 10 hits within that 1-second window, the system triggers dynamic extension",
      "End time is extended by the lot's configured extension minutes",
      "System broadcasts event TIME_EXTENDED to all participants in real time"
    ],
    "business_rules": {
      "application_scope": "Auction Timer - Anti-Sniping",
      "user_role": "Bidder, Auction Manager",
      "platform": "Web"
    },
    "acceptance_criteria": [
      {
        "id": "AC-004-001",
        "given": "Auction remaining time is 45 seconds (within the 2-minute critical zone)",
        "when": "Bid velocity reaches 10 bids in a single second and lot extension is set to 2 minutes",
        "then": "System extends the countdown by exactly 2 minutes (120 seconds) and broadcasts the new end time via WebSocket"
      },
      {
        "id": "AC-004-002",
        "given": "Auction remaining time is 45 seconds",
        "when": "Bid velocity is only 4 bids in that second",
        "then": "System accepts the bids normally without extending the timer"
      },
      {
        "id": "AC-004-003",
        "given": "Auction remaining time is 5 minutes (outside the 2-minute critical zone)",
        "when": "Bid velocity reaches 15 bids in a single second",
        "then": "System accepts the bids normally without triggering anti-sniping extension"
      }
    ]
  },
  {
    "id": "US-005",
    "title": "US-005 - Instant Buy-Out on a Capped Value Auction",
    "description": "As a Bidder, I want to immediately purchase a luxury asset in a Capped Value auction by selecting the Buy-Out option or meeting the valuation ceiling, so that the auction concludes immediately without waiting for the window to expire.",
    "preconditions": [
      "Auction is of Capped Value type with a valuation ceiling configured",
      "Bidder has available balance >= valuation ceiling"
    ],
    "business_flow": [
      "Bidder clicks the 'Instant Buy-Out' button",
      "System validates available balance >= ceiling price",
      "System locks status to 'CLOSED_AWARDED', halts countdown timer, designates Bidder as winner, and triggers instant settlement"
    ],
    "business_rules": {
      "application_scope": "Bidding - Capped Value Auction",
      "user_role": "Bidder",
      "platform": "Web"
    },
    "acceptance_criteria": [
      {
        "id": "AC-005-001",
        "given": "A Capped auction with ceiling of IDR 500,000,000 and Bidder available balance of IDR 550,000,000",
        "when": "Bidder clicks 'Instant Buy-Out'",
        "then": "Auction closes immediately, status becomes 'CLOSED_AWARDED', Bidder is declared winner, and all observers receive AUCTION_BUYOUT_CLOSED"
      },
      {
        "id": "AC-005-002",
        "given": "A Capped auction with ceiling of IDR 500,000,000 and Bidder available balance of IDR 400,000,000",
        "when": "Bidder views the auction item",
        "then": "The Buy-Out button is disabled with message: 'Insufficient balance for Buy-Out'"
      }
    ]
  },
  {
    "id": "US-006",
    "title": "US-006 - Automated Proxy Bidding",
    "description": "As a Bidder, I want to set an automatic maximum proxy bid limit, so that the system raises bids incrementally on my behalf whenever I am outbid, up to my personal limit.",
    "preconditions": [
      "Bidder is logged in",
      "Bidder available balance >= proxy maximum limit"
    ],
    "business_flow": [
      "Bidder specifies the proxy maximum limit and activates the proxy",
      "System holds collateral equal to the proxy limit",
      "When a competing bid is accepted, the background worker automatically submits the next allowed increment for this bidder",
      "If the proxy limit is surpassed, the proxy stops and the bidder is notified"
    ],
    "business_rules": {
      "application_scope": "Bidding - Proxy Bid",
      "user_role": "Bidder",
      "platform": "Web"
    },
    "acceptance_criteria": [
      {
        "id": "AC-006-001",
        "given": "Bidder activates proxy bid with limit of IDR 450,000,000 and available balance >= IDR 450,000,000",
        "when": "A competitor raises the price to IDR 390,000,000 (minimum increment IDR 5,000,000)",
        "then": "System automatically places a bid of IDR 395,000,000 on behalf of the proxy bidder in real time"
      },
      {
        "id": "AC-006-002",
        "given": "Proxy bid is active with limit of IDR 450,000,000",
        "when": "A competitor places a bid of IDR 460,000,000",
        "then": "Proxy worker stops bidding, status marks as exhausted, and Bidder receives notification"
      }
    ]
  },
  {
    "id": "US-007",
    "title": "US-007 - Auction Lot Creation and Rule Configuration (AM Exclusive)",
    "description": "As an Auction Manager, I want to create luxury asset auction listings categorized by asset type, specifying minimum price, allowed increment steps, optional maximum buyout ceiling, window duration (e.g. 1 day), and optional anti-sniping extension minutes.",
    "preconditions": [
      "User is logged in with role 'ADMIN' (promoted by Super Admin)",
      "Bidders cannot access this creation portal"
    ],
    "business_flow": [
      "Auction Manager opens the 'Create New Auction' form",
      "AM inputs Title, Asset Category (Watches, Bags, Jewelry, Vehicles, Memorabilia), Condition, and Authenticity Certificate",
      "AM configures Open Bid (Starting Price), Allowed Increment Steps, optional Buyout Ceiling, Window Duration (e.g. 86400s), and Anti-Sniping Extension Minutes",
      "System validates that ceiling (if provided) > starting price",
      "System saves the auction to PostgreSQL and publishes live state to Redis"
    ],
    "business_rules": {
      "application_scope": "Auction Management - Catalog",
      "user_role": "Auction Manager, Super Admin",
      "platform": "Web (Admin Console)"
    },
    "acceptance_criteria": [
      {
        "id": "AC-007-001",
        "given": "An Auction Manager fills all required fields with valid parameters without a ceiling price",
        "when": "AM clicks 'Publish Lot'",
        "then": "Auction is saved as an 'UNCAPPED' time-window lot and immediately appears in the active catalog for all participants"
      },
      {
        "id": "AC-007-002",
        "given": "An Auction Manager configures a Capped auction",
        "when": "Ceiling price is set less than or equal to the starting price",
        "then": "System rejects with HTTP 400: 'Valuation ceiling must be strictly greater than starting price'"
      }
    ]
  },
  {
    "id": "US-008",
    "title": "US-008 - Universal Live Bid & Concurrency Monitoring Stream",
    "description": "As a Bidder, Auction Manager, or Super Admin, I want to monitor the live bid stream and auction status in real time, so that Bidders can track market competition, AMs can oversee lot operations, and Super Admins can verify system integrity.",
    "preconditions": [
      "There is an active auction",
      "User is connected via WebSocket to ws://localhost:8080/api/v1/ws/auctions"
    ],
    "business_flow": [
      "User views the auction page / monitoring stream",
      "When a new bid is placed, the WebSocket Hub pushes event PRICE_UPDATED",
      "UI updates current price, leading bidder, total bid count, and countdown without manual page refresh"
    ],
    "business_rules": {
      "application_scope": "Live Monitoring - Multi-Role",
      "user_role": "Bidder, Auction Manager, Super Admin",
      "platform": "Web"
    },
    "acceptance_criteria": [
      {
        "id": "AC-008-001",
        "given": "A Bidder, Auction Manager, and Super Admin are all viewing the same auction lot",
        "when": "A new valid bid is committed to Redis",
        "then": "All three users receive the PRICE_UPDATED WebSocket frame simultaneously in < 300 ms"
      }
    ]
  },
  {
    "id": "US-009",
    "title": "US-009 - Automated Settlement and Collateral Reconciliation",
    "description": "As an Auction Manager, I want to oversee the automated settlement process when an auction ends, ensuring the winner's held funds are debited, losing participants' held funds are fully released, and asset ownership is legally recorded.",
    "preconditions": [
      "Auction countdown has expired or Instant Buy-Out has occurred",
      "Settlement daemon acquires the distributed lock"
    ],
    "business_flow": [
      "Settlement worker detects expired lot via Redis sorted set",
      "System locks settlement execution with Redis SETNX",
      "System debits the held collateral from the winner into permanent settlement",
      "System releases 100% of held collateral for all losing participants back to available balance",
      "System records asset ownership in PostgreSQL and broadcasts event AUCTION_CONCLUDED"
    ],
    "business_rules": {
      "application_scope": "Settlement & Reconciliation",
      "user_role": "Auction Manager, Super Admin",
      "platform": "Web"
    },
    "acceptance_criteria": [
      {
        "id": "AC-009-001",
        "given": "An auction concludes with a designated winning Bidder",
        "when": "Settlement is executed",
        "then": "Winner's locked balance is debited, asset ownership is recorded in asset_ownership, and all losing bidders have their locked balances returned 100% to available balance"
      },
      {
        "id": "AC-009-002",
        "given": "Two settlement worker instances detect expiration simultaneously",
        "when": "Both attempt to run settlement",
        "then": "Only one acquires the distributed lock; no double debit or duplicated asset transfer occurs"
      }
    ]
  }
]
```
