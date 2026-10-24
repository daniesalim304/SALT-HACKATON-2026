# User Stories & Acceptance Criteria (Fully Synchronized Baseline)

| Metadata | Nilai |
|---|---|
| **Dokumen** | Fully Synchronized User Stories & Acceptance Criteria (17 Stories) |
| **Proyek** | Real-Time Auction & Bidding Engine (Fintech / E-Commerce) |
| **Versi** | v3.0.0 (Comprehensive Baseline) |
| **Status** | `Approved Baseline` |
| **Tanggal** | 2026-10-24 |
| **Bahasa** | English & Bahasa Indonesia |

---

## 1. Ringkasan Eksekutif & Matrix Story Sizing

Seluruh 17 modul User Story dari file `Real_Time_Auction_Engine_Task_Breakdown_Sizing.xlsx` telah disinkronkan secara presisi mencakup penilaian **New Technology**, **# Surrounding**, **Technical Flow**, **Acceptance Criteria**, dan **UI Slicing**:

| ID | User Story Title | New Tech | Surr | Tech Flow | AC | UI | Total Score | Size | AC Count |
|:---|:---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **US-AUTH** | US-AUTH - Multi-Role Authentication with Progressive Lockout | 15 | 30 | 60 | 50 | 0 | **155** | **M** | 8 |
| **US-001** | US-001 - Top Up E-Wallet Balance (Bidder Exclusive & Step-by-Step UI) | 15 | 15 | 60 | 25 | 0 | **115** | **M** | 9 |
| **US-002** | US-002 - Dynamic Collateral Hold During Bidding | 45 | 15 | 90 | 25 | 0 | **175** | **M** | 4 |
| **US-003** | US-003 - Place Incremental Bid on an Uncapped Window Auction | 15 | 15 | 90 | 50 | 0 | **170** | **M** | 8 |
| **US-004** | US-004 - Velocity-Based Anti-Sniping Dynamic Timer Extension | 15 | 15 | 60 | 25 | 0 | **115** | **M** | 3 |
| **US-005** | US-005 - Instant Buy-Out on a Capped Value Auction | 15 | 15 | 60 | 25 | 0 | **115** | **M** | 3 |
| **US-006** | US-006 - Automated Proxy Bidding | 0 | 15 | 90 | 25 | 0 | **130** | **M** | 4 |
| **US-007** | US-007 - Auction Lot Creation, Rule Configuration & Form Layout (AM Exclusive) | 15 | 15 | 60 | 25 | 0 | **115** | **M** | 11 |
| **US-008** | US-008 - Universal Live Bid & Concurrency Monitoring Stream | 15 | 15 | 60 | 50 | 0 | **140** | **M** | 6 |
| **US-009** | US-009 - Automated Settlement & Reconciliation Queue (AM Exclusive) | 15 | 15 | 120 | 50 | 0 | **200** | **M** | 6 |
| **US-010** | US-010 - View Item Marketplace & Catalog Filtering | 15 | 15 | 60 | 75 | 0 | **165** | **M** | 12 |
| **US-011** | US-011 - Participant Bid History & Settlement Tracking | 15 | 15 | 60 | 50 | 0 | **140** | **M** | 10 |
| **US-012** | US-012 - Product Detail Page & Live SSE Room Integration | 15 | 15 | 30 | 50 | 0 | **110** | **M** | 9 |
| **US-013** | US-013 - Operations & Market Overview Dashboard | 0 | 15 | 0 | 75 | 0 | **90** | **S** | 6 |
| **US-014** | US-014 - View Lot Management List & Catalog Lifecycle (AM Exclusive) | 0 | 15 | 0 | 50 | 0 | **65** | **S** | 8 |
| **US-015** | US-015 - AM Live Auction Monitor & Authorized Interventions | 15 | 15 | 60 | 50 | 0 | **140** | **M** | 7 |
| **US-016** | US-016 - Participants Directory & KYC Compliance Ledger (AM Exclusive) | 15 | 15 | 60 | 50 | 0 | **140** | **M** | 6 |

---

## 2. Koleksi Lengkap 17 User Story & Acceptance Criteria (JSON Format)

```json
[
  {
    "id": "US-AUTH",
    "title": "US-AUTH - Multi-Role Authentication with Progressive Lockout",
    "description": "As a system user (Bidder, Auction Manager, or Super Admin), I want to log in using the authentication method that matches my role (NIRO for Bidder and Auction Manager, Email and Password for Super Admin) so that I can securely access the workspace for my role, and the system rejects any access that does not comply with the rules.",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 2,
      "surrounding_weight": 15,
      "tech_flow_score": 2,
      "tech_flow_weight": 30,
      "ac_score": 2,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 155,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 30,
      "tech_flow_total": 60,
      "ac_total": 50,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-AUTH-001",
        "given": "A Bidder user is on the Sign in page",
        "when": "The user clicks 'Login with NIRO' and successfully authenticates",
        "then": "The system issues a JWT session with role 'BIDDER' and redirects the user to the Bidder Workspace",
        "technical_flow": "Login Flow\r\nFE -> BE: Authorize URL Redirect\r\nBE -> FE: Return Login URL\r\nFE: Redirect and Login to Receive Niro Token\r\nFE -> BE: Auth Login Token"
      },
      {
        "id": "AC-AUTH-002",
        "given": "An Auction Manager user is on the Sign in page",
        "when": "The user clicks 'Login with NIRO' and successfully authenticates",
        "then": "The system recognizes their promoted 'ADMIN' role and redirects the user to the Auction Manager Workspace (not the Bidder workspace)",
        "technical_flow": "BE -> NIRO: Validate and Get User Info based Niro Login Token"
      },
      {
        "id": "AC-AUTH-003",
        "given": "A Bidder or Auction Manager user is on the Sign in page",
        "when": "The user attempts to submit the Email & Password form",
        "then": "The system rejects the login attempt, does not create a session, and displays: 'Please use Login with NIRO for your account type",
        "technical_flow": "Cek Registration Status\r\n== IF Registered\r\nBE: Get User Info"
      },
      {
        "id": "AC-AUTH-004",
        "given": "A Super Admin user is on the Sign in page",
        "when": "The user inputs a valid Email and Password (min. 12 characters) and submits",
        "then": "The system issues a JWT session with role 'SUPERADMIN', resets any failed login counters, and redirects to the Super Admin Portal",
        "technical_flow": "== IF Not Registered\r\nBE: Insert User Info & Insert Wallet Info"
      },
      {
        "id": "AC-AUTH-005",
        "given": "A Super Admin user is on the Sign in page",
        "when": "The user enters an incorrect password 3 consecutive times",
        "then": "The system locks the Super Admin login for 1 minute (60 seconds) and displays HTTP 429 with retry-after information",
        "technical_flow": "BE -> FE: Return Login with Token"
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
        "then": "The user fails another 3 consecutive login attempts"
      },
      {
        "id": "AC-AUTH-008",
        "given": "A Super Admin is logged in to the Super Admin Portal",
        "when": "The Super Admin selects an existing NIRO user and assigns the role 'ADMIN'",
        "then": "The user's role is updated in the database, granting them immediate access to the Auction Manager Console on subsequent logins"
      }
    ]
  },
  {
    "id": "US-001",
    "title": "US-001 - Top Up E-Wallet Balance (Bidder Exclusive & Step-by-Step UI)",
    "description": "As a Bidder, I want to top up my e-wallet balance with a minimum amount of IDR 100,000, so that I have sufficient available balance to participate in and place bids on luxury auctions",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 2,
      "tech_flow_weight": 30,
      "ac_score": 1,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 115,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 15,
      "tech_flow_total": 60,
      "ac_total": 25,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-001-001",
        "given": "The Bidder is on Step 1 (Amount) of the top-up journey.",
        "when": "The Bidder manually types an amount less than the minimum required threshold (e.g., 25.000).",
        "then": "• The system blocks the progress workflow.\r\n• The input field displays a red border wrapper outline.\r\n• A red inline validation text error message is displayed directly underneath the input box: \"Minimum top up is IDR 100,000\".\r\n• The primary \"Continue\" button remains entirely disabled.",
        "technical_flow": "FE -> BE: GET /wallet/me\r\nBE -> DB/Redis: Fetch liquid available_balance and locked_balance (float64)"
      },
      {
        "id": "AC-001-002",
        "given": "The Bidder is on Step 1 (Amount) of the top-up journey.",
        "when": "The Bidder inputs a non-standard currency value or fractions that are not whole rupiah multiples (e.g., 75.000,004).",
        "then": "• The system triggers real-time data inspection.\r\n• The input field border changes to a distinct red warning state color.\r\n• A red system message box appears stating: \"Minimum IDR 100.000 / per top-up, whole rupiah only.\".\r\n• An explanatory contextual warning block details: \"Note: our platform reserves 10% of the bid from your Available balance, so Rp1.000.000 bid will lock up to Rp1.100.000 from your account.\"",
        "technical_flow": "FE -> BE: POST /wallet/topup {amount >= 100000}\r\nBE: Validate role and Amount"
      },
      {
        "id": "AC-001-003",
        "given": "The Bidder has successfully passed Step 1 with a valid value and has transitioned into the Step 2 (Method) configuration view.",
        "when": "The Bidder attempts to click the primary action button \"Continue to payment\" before explicitly checking any Virtual Account or E-Wallet radio component cards.",
        "then": "• The system halts submission routing execution logic.\r\n• A high-priority red alert warning card notification panel appears at the top center of the main body workspace container stating: \"Choose a payment method\".\r\n• Focus is dynamically directed toward the payment channels selection field grid.",
        "technical_flow": "BE: INSERT INTO ledger_entries (type: WALLET_TOPUP)"
      },
      {
        "id": "AC-001-004",
        "given": "The user is authenticated and logged in as an Auction Manager or Super Admin.",
        "when": "The user attempts to hit the top-up endpoint or access the /profile/wallet/topup page layout",
        "then": "• The system denies routing access.\r\n• The API layer rejects the transaction with an access error message: \"Wallet top-up is exclusive to Bidder accounts.\"",
        "technical_flow": "BE -> FE: Return 200 OK with updated balance envelope"
      },
      {
        "id": "AC-001-005",
        "given": "The Bidder is on Step 3a (Pay) and has initiated a Virtual Account or E-wallet checkout flow.",
        "when": "The system payment gateway processing core encounters an unhandled system exception or returns a failure status.",
        "then": "• A high-visibility red error alert notification card appears at the top center stating: \"Payment processor failed\".\r\n• An inline sub-text message appears instructing the user to double-check the billing parameters or switch to alternative payment channels.\r\n• The primary action button resets to a retryable active state.",
        "technical_flow": "FE: Update Wallet header chips & trigger success toast notification"
      },
      {
        "id": "AC-001-006",
        "given": "The Bidder is on Step 3c (OVO Push Notification validation or security token verification view).",
        "when": "The Bidder inputs an incorrect or expired verification token security PIN sequence.",
        "then": "• The input field displays a bright red structural error outline color.\r\n• A red inline alert banner is dynamically injected stating: \"Invalid authentication code\".\r\n• The system locks validation processing execution until a fresh secure token is requested or re-entered."
      },
      {
        "id": "AC-001-007",
        "given": "The Bidder is on any Step 3 payment processing layout with a countdown clock component.",
        "when": "The countdown timer reaches the zero threshold boundary (00:00:00) before transaction verification occurs.",
        "then": "• A yellow warning alert notice box appears declaring: \"Payment session expired\".\r\n• The interface restricts direct payment submission execution logic.\r\n• A primary utility option button labeled \"Restart transaction\" or \"Re-create top up\" becomes available to safely rebuild the invoice parameters."
      },
      {
        "id": "AC-001-008",
        "given": "The Bidder is waiting on the Step 3e checking/processing screen layout.",
        "when": "The active secure network socket handshake encounters an unexpected connection breakdown state.",
        "then": "• A high-priority yellow caution banner pops up inside the workspace declaring: \"Connection lost. Re-establishing link...\".\r\n• The submission status handler disables actions to strictly block users from submitting duplicate requests.\r\n• A dynamic loader attempts to re-verify status seamlessly without dropping user transaction state history data."
      },
      {
        "id": "AC-001-009",
        "given": "The Bidder attempts to finalize an automatic direct debit account settlement link pipeline channel.",
        "when": "The chosen institution engine reports a structural institutional ceiling check failure or account insufficiency response.",
        "then": "• A red system alert banner component is mounted to the view dashboard: \"Account balance insufficient or payment channel limit exceeded.\".\r\n• The specific bank card item profile updates into a critical system warning mode styling border layout.\r\n• The workflow redirects options back to alternative payment method interfaces."
      }
    ]
  },
  {
    "id": "US-002",
    "title": "US-002 - Dynamic Collateral Hold During Bidding",
    "description": "As a Bidder, I want my available balance to be validated and held as collateral upon submitting a bid, and to remain held throughout the entire auction period until settlement, so that all bids are backed by real funds",
    "story_sizing": {
      "new_tech_score": 3,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 3,
      "tech_flow_weight": 30,
      "ac_score": 1,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 175,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 45,
      "surrounding_total": 15,
      "tech_flow_total": 90,
      "ac_total": 25,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-002-001",
        "given": "Bidder has available balance of IDR 100,000 and locked balance of IDR 0",
        "when": "Bidder places a bid of IDR 75,000",
        "then": "Bid is accepted, IDR 75,000 is moved to locked balance, and available balance becomes IDR 25,000",
        "technical_flow": "FE -> BE: Submit Increment Bid Action\r\nBE -> Redis: Verify available_balance >= target_bid"
      },
      {
        "id": "AC-002-002",
        "given": "Bidder has an active held bid of IDR 75,000",
        "when": "Another participant submits a higher bid of IDR 85,000 (outbid event)",
        "then": "The initial bidder's IDR 75,000 remains in locked balance and is NOT immediately released during the ongoing auction period",
        "technical_flow": "Redis: Atomically deduct available_balance & increase locked_balance"
      },
      {
        "id": "AC-002-003",
        "given": "Bidder has available balance of IDR 25,000",
        "when": "Bidder attempts to click a bid option requiring IDR 50,000",
        "then": "The button is disabled with an explanatory message: 'Insufficient available balance. Please top up",
        "technical_flow": "== IF Outbid Event Occurs:\r\nRedis: Update auction highest_bidder_id\r\nRedis: Retain previous bidder held funds in locked_balance (NO release during active auction)"
      },
      {
        "id": "AC-002-004",
        "given": "The auction concludes and the Bidder did not win",
        "when": "The settlement daemon processes finality",
        "then": "The Bidder's held IDR 75,000 is released 100% back to available balance",
        "technical_flow": "== IF Auction Concludes:\r\nBE Settlement: Permanently debit winner locked_balance\r\nBE Settlement: Release 100% held funds for all losing participants to available_balance"
      }
    ]
  },
  {
    "id": "US-003",
    "title": "US-003 - Place Incremental Bid on an Uncapped Window Auction",
    "description": "As a Bidder, I want to place manual bids using pre-configured increment buttons on high-value asset categories without typing free text, competing within a time window defined by the Auction Manager (e.g. 1 day).",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 3,
      "tech_flow_weight": 30,
      "ac_score": 2,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 170,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 15,
      "tech_flow_total": 90,
      "ac_total": 50,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-003-001",
        "given": "A user is logged in as a Bidder,\r\nViewing a high-value asset detail page with a Current Price of IDR 350,000,000,\r\nAnd the allowed pre-configured increments are +IDR 5m and +IDR 10m",
        "when": "The Bidder clicks the +IDR 10m button",
        "then": "• The New Price automatically updates to IDR 360,000,000 without any free text typing.\r\n• the Bidder becomes the Highest Bidder.\r\n• The price updates in real-time for all observers in < 300 ms.",
        "technical_flow": "FE: Bidder clicks pre-configured discrete increment button (+Rp 5m / +Rp 10m)"
      },
      {
        "id": "AC-003-002",
        "given": "A user is logged in as a Bidder and has clicked an increment button",
        "when": "The system triggers a Bid Confirmation Pop-up showing the exact bid summary",
        "then": "• The primary action button on the pop-up processes the official bid submission.\r\n• A close or cancel option is provided to dismiss the action without placing a bid.\r\n• The modal must display the correct Asset Title (\"Vintage Rolex Submariner 1968\").\r\n• It must clearly show the Bid Amount being placed (IDR 360,000,000).\r\n• It must list the Bidder's Virtual Account / Wallet Info for verification.",
        "technical_flow": "FE -> BE: POST /api/v1/auctions/:id/bids"
      },
      {
        "id": "AC-003-003",
        "given": "The \"Confirm your bid\" Pop-up modal is actively displayed on the Bidder's screen",
        "when": "The Bidder clicks the \"Confirm Bid\" button (primary blue button)",
        "then": "• The pop-up closes immediately.\r\n• The system processes the bid submission.\r\n• The main screen background updates smoothly to reflect the new highest bidder status.",
        "technical_flow": "BE: Validate increment matches allowed_increment_steps (Reject free text input)"
      },
      {
        "id": "AC-003-004",
        "given": "The \"Confirm your bid\" Pop-up modal is actively displayed on the Bidder's screen",
        "when": "The Bidder clicks the \"Cancel\" / \"Back\" button or the \"X\" icon",
        "then": "• The pop-up closes immediately.\r\n• No bid is submitted to the system.\r\n• The main auction page remains in its exact previous state with no changes to the current price.",
        "technical_flow": "BE -> Redis: EVALSHA bid_placement.lua\r\nRedis: Check wallet available_balance >= new_price\r\nRedis: Atomic mutation of current_price & highest_bidder_id"
      },
      {
        "id": "AC-003-005",
        "given": "The \"Confirm your bid\" Pop-up modal is actively displayed on the screen",
        "when": "The live auction countdown timer is ticking down in the background",
        "then": "• The remaining time window (e.g., 00:04:32) must remain visible or sync correctly inside/behind the modal.\r\n• If the time window expires (00:00:00) while the pop-up is still open, the modal must automatically close and show a \"Time Expired\" alert to prevent late submission.",
        "technical_flow": "Redis -> PubSub: Publish internal price_updated event"
      },
      {
        "id": "AC-003-006",
        "given": "Two distinct Bidders are viewing the same active auction page",
        "when": "Both Bidders click the increment button simultanesouly",
        "then": "• The system processes both incoming requests via Redis Lua atomic serialization.\r\n• The first arriving bid is accepted.\r\n• The second arriving bid is immediately evaluated against the newly incremented price with zero race condition or duplicate data errors.",
        "technical_flow": "BE SSE Hub: Broadcast PRICE_UPDATED payload to all connected clients in < 300 ms"
      },
      {
        "id": "AC-003-007",
        "given": "A user is logged in as an Auction Manager or Super Admin",
        "when": "Viewing the auction detail page",
        "then": "• The manual bid action buttons (Quick Increment Buttons & Place Bid Column) are completely hidden or disabled.\r\n• This strictly prevents any internal conflict of interest during the live auction.",
        "technical_flow": "BE -> DB: Asynchronously append bid log into bids table"
      },
      {
        "id": "AC-003-008",
        "given": "A user is logged in as a Bidder on the auction detail page",
        "when": "Observing the remaining time on the Countdown Timer component",
        "then": "• Bidders can only compete within the specific time window defined by the Auction Manager (e.g., 1 day).\r\n• Once the timer hits 00:00:00, all bidding actions are automatically locked out in real-time."
      }
    ]
  },
  {
    "id": "US-004",
    "title": "US-004 - Velocity-Based Anti-Sniping Dynamic Timer Extension",
    "description": "As a Bidder and Auction Manager, I want the auction timer to automatically extend if high bidding velocity (>= 10 hits/second) is detected in the final 2 minutes, using the extension duration configured for that lot",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 2,
      "tech_flow_weight": 30,
      "ac_score": 1,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 115,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 15,
      "tech_flow_total": 60,
      "ac_total": 25,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-004-001",
        "given": "Auction remaining time is 45 seconds (within the 2-minute critical zone)",
        "when": "Bid velocity reaches 10 bids in a single second and lot extension is set to 5 minutes",
        "then": "System extends the countdown by exactly 5 minutes and broadcasts the new end time via WebSocket",
        "technical_flow": "BE Redis Lua: Check remaining_time <= 120 seconds (Last 2-Minute Critical Zone)"
      },
      {
        "id": "AC-004-002",
        "given": "Auction remaining time is 45 seconds",
        "when": "Bid velocity is only 4 bids in that second",
        "then": "System accepts the bids normally without extending the timer",
        "technical_flow": "BE Redis: Calculate bid velocity count in rolling 1-second sliding window"
      },
      {
        "id": "AC-004-003",
        "given": "Auction remaining time is 5 minutes (outside the 2-minute critical zone)",
        "when": "Bid velocity reaches 15 bids in a single second",
        "then": "System accepts the bids normally without triggering anti-sniping extension",
        "technical_flow": "== IF Velocity >= 10 bids/second:\r\nRedis: Trigger Anti-Sniping extension\r\nRedis: end_time = end_time + (extension_minutes * 60)\r\nRedis: Update ZSET score in auctions:expiry_zset"
      }
    ]
  },
  {
    "id": "US-005",
    "title": "US-005 - Instant Buy-Out on a Capped Value Auction",
    "description": "As a Bidder, I want to immediately purchase a luxury asset in a Capped Value auction by selecting the Buy-Out option or meeting the valuation ceiling, so that the auction concludes immediately without waiting for the window to expire.",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 2,
      "tech_flow_weight": 30,
      "ac_score": 1,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 115,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 15,
      "tech_flow_total": 60,
      "ac_total": 25,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-005-001",
        "given": "A Capped auction is active with a valuation ceiling of IDR 500,000,000, and the Bidder's available balance is IDR 550,000,000.",
        "when": "The Bidder clicks the 'Instant Buy-Out' button.",
        "then": "• The auction closes immediately.\r\n• The system updates the status to 'CLOSED_AWARDED'.\r\n• The Bidder is declared the winner.\r\n• All active observers receive a live AUCTION_BUYOUT_CLOSED system event.",
        "technical_flow": "FE: Bidder clicks \"Instant Buy-Out\" action button on Capped Valuation lot"
      },
      {
        "id": "AC-005-002",
        "given": "A Capped auction is active with a valuation ceiling of IDR 500,000,000, and the Bidder's available balance is only IDR 400,000,000",
        "when": "The Bidder views the auction item detail page.",
        "then": "• The 'Instant Buy-Out' button is disabled/greyed out.\r\n• The system displays a tooltip or inline message: 'Insufficient balance for Buy-Out'.",
        "technical_flow": "FE -> BE: POST /api/v1/auctions/:id/bids {buyout: true, amount: valuation_ceiling}"
      },
      {
        "id": "AC-005-003",
        "given": "The user is actively viewing the product detail page for \"Vintage Rolex Submariner 1968\" where they previously held the highest bid.",
        "when": "Another bidder places a higher bid that does not reach the ceiling value yet.",
        "then": "• The system instantly displays a red alert banner at the top right: \"You have been outbid - Current highest bid: [Amount]. Review your bid.\"\r\n• The system updates the 'My Position' badge to a red \"Outbid\" status.\r\n• The main status badge next to the product title changes to \"Outbid\" to prompt immediate re-bidding or buy-out consideration.",
        "technical_flow": "BE -> Redis: EVALSHA bid_placement.lua\r\nRedis: Validate available_balance >= valuation_ceiling\r\nRedis: Mutate lot status from ACTIVE to CLOSED_AWARDED immediately"
      }
    ]
  },
  {
    "id": "US-006",
    "title": "US-006 - Automated Proxy Bidding",
    "description": "As a Bidder, I want to set an automatic maximum proxy bid limit, so that the system raises bids incrementally on my behalf whenever I am outbid, up to my personal limit",
    "story_sizing": {
      "new_tech_score": 0,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 3,
      "tech_flow_weight": 30,
      "ac_score": 1,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 130,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 0,
      "surrounding_total": 15,
      "tech_flow_total": 90,
      "ac_total": 25,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-006-001",
        "given": "A user is logged in as a Bidder with an available wallet balance ≥ IDR 450,000,000,\r\nAnd enters a Maximum bid limit of IDR 450,000,000 in the Autonomous Bidding card",
        "when": "The Bidder toggles the switch to active and clicks the \"Start\" button",
        "then": "• The system locks/validates the maximum proxy threshold.\r\n• The card status changes to active/running with visual checklist anchors (e.g., \"Bids only when you are outbid\", \"Stops as your limit\").\r\n• The \"Start\" button turns into a red \"Stop\" button alongside a \"Pause\" option as shown in the Figma UI.",
        "technical_flow": "FE -> BE: POST /api/v1/auctions/:id/proxy-bid {max_limit: float64}\r\nBE: Validate available_balance >= max_limit\r\nBE: Store proxy rule in PostgreSQL & Redis hash"
      },
      {
        "id": "AC-006-002",
        "given": "The Bidder's proxy bid is active with a limit of IDR 450,000,000,\r\nAnd the current minimum increment is IDR 5,000,000",
        "when": "A competitor raises the asset price to IDR 390,000,000",
        "then": "• The system proxy worker instantly calculates the next valid increment.\r\n• The system automatically places a counter-bid of IDR 395,000,000 on behalf of the proxy bidder in real-time.\r\n• The proxy bidder retains the \"Highest Bidder\" status.",
        "technical_flow": "BE Proxy Worker: Subscribe to Redis PRICE_UPDATED event stream"
      },
      {
        "id": "AC-006-003",
        "given": "The Bidder's proxy bid is active with a limit of IDR 450,000,000",
        "when": "A competitor places a higher bid of IDR 460,000,000",
        "then": "• The system proxy worker immediately stops bidding.\r\n• The autonomous bidding state is marked as exhausted / inactive.\r\n• The Bidder instantly receives a push notification/alert stating they have been outbid.",
        "technical_flow": "== IF Outbid Detected AND current_price + min_step <= max_limit:\r\nProxy Worker: Calculate next increment\r\nProxy Worker: Submit automated counter-bid on behalf of user"
      },
      {
        "id": "AC-006-004",
        "given": "The Autonomous Bidding worker is actively running in the background",
        "when": "The Bidder clicks the \"Pause\" or \"Stop\" button on the UI card",
        "then": "• The system immediately halts the automated agent loop.\r\n• The interface state toggles back to editable inputs.\r\n• No further automated bids are placed unless the user re-triggers the \"Start\" flow",
        "technical_flow": "== IF current_price + min_step > max_limit:\r\nProxy Worker: Mark proxy as EXHAUSTED\r\nProxy Worker -> Notification: Dispatch Outbid push notification"
      }
    ]
  },
  {
    "id": "US-007",
    "title": "US-007 - Auction Lot Creation, Rule Configuration & Form Layout (AM Exclusive)",
    "description": "As an Auction Manager, I want to create luxury asset auction listings categorized by asset type, specifying minimum price, allowed increment steps, optional maximum buyout ceiling, window duration (e.g. 1 day), and optional anti-sniping extension minutes.",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 2,
      "tech_flow_weight": 30,
      "ac_score": 1,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 115,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 15,
      "tech_flow_total": 60,
      "ac_total": 25,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-007-001",
        "given": "An Auction Manager fills all required fields with valid parameters without a ceiling price",
        "when": "AM clicks 'Publish Lot",
        "then": "Auction is saved as an 'UNCAPPED' time-window lot and immediately appears in the active catalog for all participants",
        "technical_flow": "FE: Auction Manager fills lot specs, categories, start/end date, allowed steps, and attaches image"
      },
      {
        "id": "AC-007-002",
        "given": "An Auction Manager configures a Capped auction",
        "when": "Ceiling price is set less than or equal to the starting price",
        "then": "System rejects with HTTP 400: 'Valuation ceiling must be strictly greater than starting price",
        "technical_flow": "FE -> BE: POST /api/v1/admin/auctions (multipart/form-data)"
      },
      {
        "id": "AC-007-003",
        "given": "The Auction Manager (AM) is on the \"Operations overview\" main dashboard workspace.",
        "when": "The AM clicks the primary blue action button \"+ Create lot\" located at the top-right section of the header.",
        "then": "The system terminates the dashboard rendering state and redirects the browser workflow to the blank \"Create lot & auction\" listing creation form layout.",
        "technical_flow": "BE: Authenticate role == ADMIN (Auction Manager)"
      },
      {
        "id": "AC-007-004",
        "given": "The Auction Manager (AM) is authenticated and viewing any administrative layout workspace page.",
        "when": "The AM clicks the \"Lot Management\" menu link option available on the left-hand persistent navigation drawer menu (sidebar).",
        "then": "The system routes the view and grants the user direct access to the operational management panel flow and listing creation environment.",
        "technical_flow": "BE: Read binary image multipart stream and compress to BYTEA BLOB\r\nBE: Store image_mime_type and record to auctions table"
      },
      {
        "id": "AC-007-005",
        "given": "The AM is on the \"Create lot & auction\" workspace interface.",
        "when": "The AM enters the asset identity specifications.",
        "then": "• Lot Title: Text input handles alphanumeric names (e.g., \"Rolex GMT-Master 1675 'Radial'\").\r\n• Category: Dropdown allows filtering and selecting specific asset types (e.g., \"Watches\").\r\n• Description: Text area supports detailed asset metadata, conditions, and completeness context.",
        "technical_flow": "BE -> Redis: Initialize auction:{id} hash and insert end_time into auctions:expiry_zset"
      },
      {
        "id": "AC-007-006",
        "given": "The AM modifies any input parameters in Form Section 1 (Asset Details).",
        "when": "The data fields change state or text is typed",
        "then": "• The system dynamically renders a \"Bidder preview\" component box on the right sidebar panel.\r\n• The preview updates the Title, Category tag, and description text in real-time to mimic how it will look on the live Marketplace layout.",
        "technical_flow": "BE -> FE: Return 201 Created with Base64 compressed image preview"
      },
      {
        "id": "AC-007-007",
        "given": "The AM scrolls down to the \"Auction type & pricing\" panel block.",
        "when": "The AM sets the structural financial configurations for the lot.",
        "then": "• Auction Type: The system provides toggle options (e.g., Standard Auction vs Capped/Ceiling Auction).\r\n• Starting Price: Input field locks numbers and sets the baseline minimum valuation (e.g., Rp10.000.000).\r\n• Bid Increment: Input field restricts minimal validation steps (e.g., Rp10.000.000 text field).\r\n• Buy-Out Ceiling (Optional): If enabled via toggle, a maximum threshold input field appears to capture the buyout value."
      },
      {
        "id": "AC-007-008",
        "given": "The AM proceeds to the \"Schedule & extension\" panel block.",
        "when": "The AM sets the operational timing limits for the auction lifecycle",
        "then": "• Start & End Time: Input fields provide integrated date-time picker selectors.\r\n• Anti-Sniping Extension: A dropdown menu allows selecting anti-snipe buffer minutes (e.g., \"Choose Time Extension\" options list: 2 Minutes, 5 Minutes, 10 Minutes, 15 Minutes)."
      },
      {
        "id": "AC-007-009",
        "given": "The AM proceeds to the \"Eligibility & collateral\" block at the bottom of the form.",
        "when": "The AM defines security gate parameters.",
        "then": "• Eligible Bidders: Dropdown menu controls baseline identity restrictions (e.g., \"Verified bidders only\").\r\n• Collateral Hold: A numerical or percentage input defines the frozen balance threshold needed to join (e.g., \"10% of standard bid value\")."
      },
      {
        "id": "AC-007-010",
        "given": "The AM has populated fields in the creation workspace.",
        "when": "The AM attempts to click the \"Cancel\" action button or navigate away via the sidebar menus before saving.",
        "then": "• The system halts the routing execution.\r\n• A confirmation modal dialog wrapper is mounted to the UI dashboard warning the user about unsaved progress loss.\r\n• Confirming cancellation clears state; cancelling modal returns the user to the form with full text inputs intact."
      },
      {
        "id": "AC-007-011",
        "given": "The AM fills out all mandatory asterisk (*) fields with valid data values.",
        "when": "The AM clicks the primary header button \"Review & publish\".",
        "then": "• The system validates inputs and triggers the database transaction orchestration layer.\r\n• A success notification banner appears.\r\n• The system automatically indexes the lot into the public listing inventory and updates the \"Operations Overview\" macro counter stats."
      }
    ]
  },
  {
    "id": "US-008",
    "title": "US-008 - Universal Live Bid & Concurrency Monitoring Stream",
    "description": "As a Bidder I want to monitor the live bid stream and auction status in real time, so that Bidders can track market competition, AMs can oversee lot operations, and Super Admins can verify system integrity",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 2,
      "tech_flow_weight": 30,
      "ac_score": 2,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 140,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 15,
      "tech_flow_total": 60,
      "ac_total": 50,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-008-001",
        "given": "The user (Bidder, AM, or Super Admin) is logged into the Auction Engine platform and opens the Overview Dashboard.",
        "when": "The system establishes an active WebSocket/live network connection.",
        "then": "• The top header status pill component must dynamically render a green indicator showing \"● Live • synced\".\r\n• All numerical metrics, countdown timers, and tables must update asynchronously without requiring a manual browser refresh.",
        "technical_flow": "FE: Client initializes EventSource connection to /api/v1/sse/auctions/:id"
      },
      {
        "id": "AC-008-002",
        "given": "The Bidder is viewing the top grid panel of the dashboard workspace.",
        "when": "The background live sync data framework receives new transaction events.",
        "then": "The system must instantly display and balance check four core tracking metrics:\r\n• Available Balance: Shows ready-to-use funds (e.g., Rp48.250.000) with estimated changes underneath.\r\n• Locked Collateral: Highlights funds currently frozen for active bids (e.g., Rp9.500.000).\r\n• Active Bids: Displays total active count (e.g., 8) with a red warning subtext if user is outbid (e.g., 3 outbid / 1 highest).\r\n• Won this month: Shows successful historical outcomes.",
        "technical_flow": "BE SSE Hub: Establish persistent HTTP/2 chunked response stream (text/event-stream)"
      },
      {
        "id": "AC-008-003",
        "given": "The user has items saved to their personal auction watchlist.",
        "when": "An item in the \"Ending soon on your watchlist\" slider receives a new bid or reaches a critical time state.",
        "then": "• The card overlay badge updates dynamically based on status (e.g., changing to a yellow blinking \"🔥 Ending soon\" or yellow \"⏳ Extended\" text badge).\r\n• The \"Current bid\" value flashes and updates instantly.\r\n• The \"Time Remaining\" countdown clock decrements perfectly every second.",
        "technical_flow": "BE Engine: High-concurrency serialization in Redis Lua (< 5 ms execution)"
      },
      {
        "id": "AC-008-004",
        "given": "The user (especially Bidder tracking market or AM overseeing lots) scrolls to the \"My active bids\" / \"Lot Management\" grid table.",
        "when": "Any status permutation transition occurs on a live item lot.",
        "then": "The system row must change state instantly, applying the correct color-coded visual indicator badges under the Status column:\r\n• Leading (Green): User currently holds the valid highest bid.\r\n• Outbid (Red): Another participant has overtaken the price matrix.\r\n• Extended (Orange): Anti-snipe time rule extension rules have kicked in.\r\n• Active (Blue): Open for standard bidding interaction.\r\n• Scheduled (Grey): Future lot item not yet open for live bids.",
        "technical_flow": "BE SSE Hub: Multiplex & dispatch price mutation frames to all active observers simultaneously"
      },
      {
        "id": "AC-008-005",
        "given": "The Super Admin, AM, or Bidder looks at the \"Recent activity\" timeline list at the bottom container area",
        "when": "Any transaction, system automated action, or network anomaly event is broadcasted.",
        "then": "The system must immediately append chronological system event lines with specific descriptive logging badges:\r\n• Bid Success (Green check): \"Your bid Rp12.500.000 was accepted\".\r\n• Outbid Notice (Red caution): \"You have been outbid\".\r\n• Auto-Bid Automation (Purple lightning): \"Auto-bid executed Rp12.400.000\".\r\n• Time Extension Rule (Yellow clock): \"Auction extended by 2 minutes\".",
        "technical_flow": "Prometheus Exporter: Scrape telemetry & confirm latency SLA < 300 ms under 10k req/s load"
      },
      {
        "id": "AC-008-006",
        "given": "The system ledger detects that an active logged-in user has won an item but has not settled the invoice block",
        "when": "The Overview dashboard page finishes loading",
        "then": "• A high-priority blue systemic notice banner appears at the very top workspace grid section stating: \"Payment due for 1 won auction. Settle by [Date/Timestamp]. Balance must clear...\".\r\n• A functional \"Pay now\" action button is displayed on the far right edge of the alert element box to bypass and speed up the checkout settlement routing."
      }
    ]
  },
  {
    "id": "US-009",
    "title": "US-009 - Automated Settlement & Reconciliation Queue (AM Exclusive)",
    "description": "As an Auction Manager, I want to oversee the automated settlement process when an auction ends, ensuring the winner's held funds are debited, losing participants' held funds are fully released, and asset ownership is legally recorded.\"",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 4,
      "tech_flow_weight": 30,
      "ac_score": 2,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 200,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 15,
      "tech_flow_total": 120,
      "ac_total": 50,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-009-001",
        "given": "The Auction Manager (AM) is logged into the Admin Workspace",
        "when": "The AM clicks on the \"Settlement Queue\" menu link option in the persistent sidebar",
        "then": "• The system renders the \"Settlement & reconciliation\" dashboard tracking panel.\r\n• The header sub-text must state: \"Auction result + settlement. Reconcile payments, handle exceptions, release collateral.\"\r\n• The system displays four real-time metric milestone cards: Pending payment (12), Processing (7), Exceptions (3), and Settled today (29).",
        "technical_flow": "BE Daemon: 1-second interval poller runs ZRANGEBYSCORE auctions:expiry_zset -inf now()"
      },
      {
        "id": "AC-009-002",
        "given": "The AM is reviewing the upper section of the settlement transaction database list grid",
        "when": "The AM clicks on any category tab component filter link (e.g., All, Pending, Processing, Exceptions, Settled).",
        "then": "• The list grid dynamically switches rows to display only items matching that specific operational state query.\r\n• Each tab row accurately prints corresponding record total counters matching the master stats (e.g., clicking \"Exceptions\" displays exactly 3 records).",
        "technical_flow": "BE Daemon: Acquire distributed lock SET lock:settle:{lot_id} NX EX 10"
      },
      {
        "id": "AC-009-003",
        "given": "The AM is viewing the active records grid inside the \"Exceptions\" filter tab menu container grid.",
        "when": "The AM clicks on a specific settlement invoice row item (e.g., \"STL-2026-0079 - Leica M6\").",
        "then": "• The clicked item row highlights with an active background visual selection styling container framework.\r\n• The system dynamically mounts the granular invoice deep-dive breakdown summary inspector block panel on the right margin side area workspace.",
        "technical_flow": "BE Daemon: Fetch final winner and highest bid amount from Redis"
      },
      {
        "id": "AC-009-004",
        "given": "The system loads the detailed panel view for an exception state transaction (e.g., STL-2026-0079).",
        "when": "The page finishes loading.",
        "then": "• The upper block displays a red structural error warning container component declaring: \"Amount mismatch - Rp50.000 short\".\r\n• The system prints the structural root cause audit log parameters: \"Received Rp33.100.000 via BCA VA on 22 Oct, expected Rp33.150.000.\"",
        "technical_flow": "BE -> DB: BEGIN TRANSACTION (SERIALIZABLE)\r\nDB: Debit winner locked_balance (insert AUCTION_WIN_PAYMENT)\r\nDB: Update auction status = SETTLED\r\nDB: Insert asset_ownership record\r\nDB: Release 100% held locked_balance for all losing bidders to available_balance\r\nDB: COMMIT TRANSACTION"
      },
      {
        "id": "AC-009-005",
        "given": "The AM is inspecting the visual tracking stepper component timeline inside the right-hand panel view workspace dashboard.",
        "when": "The backend background orchestrator triggers steps asynchronously.",
        "then": "The system updates the progress checkmark stepper layout color codes to map legal transitions:\r\n• ✓ Auction won (Green dot checkmark timestamp).\r\n• ✓ Collateral locked (Green dot checkmark displaying frozen funds e.g., Rp12.500.000).\r\n• ◌ Payment processing (Blue circle loader tracking gateway verification timelines e.g., Est. 1-2 business days).\r\n• ◌ Settled & ◌ Collateral released (Grey circles reflecting pending terminal milestones).",
        "technical_flow": "BE -> Redis: Clean up active lot keys & ZREM from auctions:expiry_zset"
      },
      {
        "id": "AC-009-006",
        "given": "The AM determines a path to reconcile a disputed mismatch transaction state invoice.",
        "when": "The AM triggers an action button block inside the \"Resolution\" footer interface grid component.",
        "then": "The system processes three custom override logic sequences:\r\n• \"Request Rp50.000\": Dispatches an outstanding invoice balance reminder notification to the user profile link.\r\n• \"Write off\": Waives minor variations and pushes ledger forward.\r\n• \"Mark reconciled\": Manual blue CTA button that updates invoice status records to Settled and releases losing participants' collateral holds instantly.",
        "technical_flow": "BE SSE Hub: Broadcast AUCTION_CONCLUDED event to all room participants"
      }
    ]
  },
  {
    "id": "US-010",
    "title": "US-010 - View Item Marketplace & Catalog Filtering",
    "description": "As a Bidder / Auction Participan I want to view the list of products currently up for auction So that I can select items that interest me and start placing bids",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 2,
      "tech_flow_weight": 30,
      "ac_score": 3,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 165,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 15,
      "tech_flow_total": 60,
      "ac_total": 75,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-010-001",
        "given": "The user is on the \"Marketplace\" page",
        "when": "The system successfully loads the auction product data",
        "then": "The page displays product cards containing: \r\n1. Category,\r\n2. Item Name\r\n3.  Lot ID,\r\n4. Based Price\r\n5. Latest Bid\r\n6. Number of Bids\r\n7. Countdown Timer\r\n8. Watchlist Icon (Heart), \r\n9. and a \"View Detail\" button.",
        "technical_flow": "FE: User navigates to Marketplace page"
      },
      {
        "id": "AC-010-002",
        "given": "The user opens the \"Marketplace\" page",
        "when": "The system finishes loading the page",
        "then": "The system displays the total active auction products (e.g., \"Showing 12 of 248 lots - Live updates on\") along with the last updated time indicator.",
        "technical_flow": "FE -> BE: GET /api/v1/auctions/active?page=1&limit=12&category=LUXURY_WATCHES"
      },
      {
        "id": "AC-010-003",
        "given": "The user is on the \"Marketplace\" page",
        "when": "The user clicks the \"Type\" filter dropdown (marked as F) and selects a specific type",
        "then": "The system filters and only displays products matching the selected type.",
        "technical_flow": "BE: Query auctions table, calculate pagination metadata, decode BYTEA image to Base64"
      },
      {
        "id": "AC-010-004",
        "given": "The user is viewing the marketplace product list",
        "when": "An auction product is currently in progress or ongoing",
        "then": "The system displays the product card with an \"Active\" or \"Extended\" status label and shows the remaining time countdown.",
        "technical_flow": "BE -> FE: Return standardized JSON envelope with items & pagination"
      },
      {
        "id": "AC-010-005",
        "given": "The user is viewing the marketplace product list",
        "when": "The user has placed a bid on a product and their bid amount is currently the highest",
        "then": "The system displays the product card with a \"Leading\" status label.",
        "technical_flow": "FE: Connect to SSE /api/v1/sse/catalog to receive live price ticker updates"
      },
      {
        "id": "AC-010-006",
        "given": "The user is viewing the marketplace product list",
        "when": "An ongoing auction product is close to its time limit / almost finished",
        "then": "The system displays the product card with an \"Ending Soon\" status label.",
        "technical_flow": "FE: Dynamic badge resolver (Active, Leading, Ending Soon, Extended, Scheduled)"
      },
      {
        "id": "AC-010-007",
        "given": "The user is viewing the marketplace product list",
        "when": "A product is registered for auction but the start time has not arrived yet",
        "then": "The system displays the product card with a \"Scheduled\" status label and shows the upcoming start date/time."
      },
      {
        "id": "AC-010-008",
        "given": "The user is on the first page of the marketplace",
        "when": "The user clicks the \"Next\" button at the bottom navigation",
        "then": "The system loads and displays the next set of products, and the text updates to \"Page 2 of 21\"."
      },
      {
        "id": "AC-010-009",
        "given": "The user is on page 2 or higher of the marketplace",
        "when": "The user clicks the \"Previous\" button at the bottom navigation",
        "then": "The system loads and displays the previous set of products, and the page indicator decrements accordingly."
      },
      {
        "id": "AC-010-010",
        "given": "The user is on the first page (\"Page 1 of 21\")",
        "when": "The user views the pagination controls",
        "then": "The \"Previous\" button is disabled (unclickable) and styled visually as inactive."
      },
      {
        "id": "AC-010-011",
        "given": "There are no auction products at all in the system database",
        "when": "The user opens or navigates to the \"Marketplace\" page",
        "then": "The system displays an empty state page with an informative message: \"There are currently no products up for auction\" and hides the pagination controls completely."
      },
      {
        "id": "AC-010-012",
        "given": "The user is on the \"Marketplace\" page",
        "when": "The user enters a non-existent keyword or applies invalid filter combinations",
        "then": "The system displays a \"no result found\" message and hides the pagination controls since there are no pages to display."
      }
    ]
  },
  {
    "id": "US-011",
    "title": "US-011 - Participant Bid History & Settlement Tracking",
    "description": "As a Bidder, I want to view my bid history, so that I can review the details and status of my previous bids and track my bidding activity.",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 2,
      "tech_flow_weight": 30,
      "ac_score": 2,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 140,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 15,
      "tech_flow_total": 60,
      "ac_total": 50,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-011-001",
        "given": "The user is on the \"Bid History\" page",
        "when": "The system successfully loads the data",
        "then": "The page displays summary widgets showing counts for: Active Bids, Leading, Outbid, and Won/Settled, along with descriptive sub-texts (e.g., \"X values below reserve values\").",
        "technical_flow": "FE: User navigates to Bid History page"
      },
      {
        "id": "AC-011-002",
        "given": "The user has an outstanding payment for a won auction",
        "when": "The user views the top section of the page",
        "then": "The system displays a yellow warning banner showing the payment deadline countdown (e.g., \"Payment due in 1 day 22 hours\") and total outstanding amount.",
        "technical_flow": "FE -> BE: GET /api/v1/auctions/my-bids?status=ACTIVE"
      },
      {
        "id": "AC-011-003",
        "given": "The user is on the \"Bid History\" page",
        "when": "The user clicks on one of the activity tabs (All activity, Active Bids, Won, Lost)",
        "then": "The system filters and displays only the auction lots that match the selected status tab.",
        "technical_flow": "BE: Aggregate active bids, leading lots, outbid counts, and won auctions"
      },
      {
        "id": "AC-011-004",
        "given": "The user is viewing the \"Active Bidding\" section",
        "when": "The system displays the active bidding table",
        "then": "The table must show detailed columns: Lot Info, My Bid, Highest Bid, Position Status (Leading/Outbid), Mode (Manual/Auto-Bid), Status (Countdown), and Action button (View/Bid open).",
        "technical_flow": "BE -> FE: Return aggregated counts & lot list envelope"
      },
      {
        "id": "AC-011-005",
        "given": "The user scrolls down to the \"Closed Auctions\" section",
        "when": "The system populates the ended auction list",
        "then": "Each closed lot displays a visual step-by-step progress tracking bar with circles for: Won, Payment, Verification, and Collateral Released.",
        "technical_flow": "FE: User clicks a specific closed auction item"
      },
      {
        "id": "AC-011-006",
        "given": "The user clicks or highlights a specific closed auction item",
        "when": "The side panel dynamically loads the data",
        "then": "The system displays the \"Settlement detail\" section on the right side, showing Hammer price, Buyer premium, Total cost, Collateral applied, Amount to pay, Due by date, and the chronological status steps (e.g., Auction won -> Collateral locked -> Payment processing).",
        "technical_flow": "FE -> BE: Fetch settlement details (Hammer price, Buyer premium, Collateral applied)"
      },
      {
        "id": "AC-011-007",
        "given": "The user is viewing a settlement detail that requires payment",
        "when": "The user clicks the solid blue \"Pay [Amount]\" button at the bottom of the sidebar",
        "then": "The system redirects the user to the payment gateway or checkout page to settle the transaction.",
        "technical_flow": "FE: Settle button redirects to payment/checkout if balance due exists"
      },
      {
        "id": "AC-011-008",
        "given": "The user has never participated in any auction or bidding activity",
        "when": "The user navigates to the \"Bid History\" page",
        "then": "The system displays an empty state illustration with an informative text: \"You have no bidding history yet.\" and summary widgets show a count of 0."
      },
      {
        "id": "AC-011-009",
        "given": "The user applies multi-layered dropdown filters (Category, Type, Bid status, Price range)",
        "when": "The chosen filter combination does not match any historical data",
        "then": "The system displays an empty table layout with a message: \"No bid history matches your active filters.\""
      },
      {
        "id": "AC-011-010",
        "given": "The user is actively monitoring countdowns on the table",
        "when": "The user's internet connection is lost",
        "then": "The system halts live counter updates and displays a toast alert message: \"Connection lost. Retrying to sync your bid activities...\" while keeping existing data on screen."
      }
    ]
  },
  {
    "id": "US-012",
    "title": "US-012 - Product Detail Page & Live SSE Room Integration",
    "description": "As a Bidder, I want to view product details, so that I can review the product information and evaluate the product before placing a bid.",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 1,
      "tech_flow_weight": 30,
      "ac_score": 2,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 110,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 15,
      "tech_flow_total": 30,
      "ac_total": 50,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-012-001",
        "given": "The user is on the Marketplace page (Bidding Entry Point) and selects/clicks on a specific product item (e.g., \"Vintage Rolex Submariner 1968\").",
        "when": "The user selects the item.",
        "then": "The system redirects the user to the product detail page, displaying the specific information of the selected item (LOT-2828-8142).",
        "technical_flow": "1. [Detail] Implement Auction Product Detail Endpoint\r\n- Route GET /api/v1/auctions/:id\r\n- Return full Base64 compressed image, authenticity guarantee, and specs\r\n- Return allowed_increment_steps array"
      },
      {
        "id": "AC-012-002",
        "given": "The user is successfully redirected and is on the product detail page.",
        "when": "The page finishes loading.",
        "then": "The system must display:\r\n• Product Name: \"Vintage Rolex Submariner 1968\"\r\n• Auction Status: \"Active\" and \"Bidding\"\r\n• Time Countdown: Remaining auction time running (visible yellow box \"Time remaining 00:04:32\")\r\n• Current Price: \"Current highest bid Rp12.500.000\" along with highest bidder info.",
        "technical_flow": "2. [SSE] Real-Time Room Event Listener & State Synchronization\r\n- Connect to /api/v1/sse/auctions/:id EventSource\r\n- Update current price, bidder rank, countdown timer, and live price chart"
      },
      {
        "id": "AC-012-003",
        "given": "The user is on the product detail page and scrolls to the product information area.",
        "when": "The page finishes loading.",
        "then": "The system displays the product detail information consisting of:\r\n• Menu Tabs: Details, Biders, Bid History, Documents.\r\n• About this lot: Description of product completeness.\r\n• Auction Rules: Displays Auction type, Starting price, Bid increment, Extension rule, and Lot ID.",
        "technical_flow": "3. [Delivery] Product Detail Page Slicing & Offline Banner\r\n- Tabs layout (Details, Bidders, Bid History, Documents)\r\n- Connection interrupted banner with \"Retry now\" reconnect action"
      },
      {
        "id": "AC-012-004",
        "given": "The user is on the product detail page and looks below the description section.",
        "when": "The page finishes loading.",
        "then": "The system displays the Price History chart (bid price increase trend) complete with bidding timestamps and the total number of bids submitted."
      },
      {
        "id": "AC-012-005",
        "given": "The user is on the product detail page at the bottom section of the price history.",
        "when": "The page finishes loading",
        "then": "The system displays the latest Bid History list in real-time, including:\r\n• Bidding actions (e.g., \"You bid Rp12.000.000\")\r\n• Automatic time extension status (\"Auction extended by 2 minutes\")\r\n• Internet connection status (\"Connection restored\")"
      },
      {
        "id": "AC-012-006",
        "given": "The user is on the product detail page and looks at the interaction panel on the right side.",
        "when": "The user wants to place a price bid.",
        "then": "The system displays the Place Bid form containing:\r\n• Bid amount input field (automatically pre-filled with the recommended minimum next bid, e.g., Rp12.600.000)\r\n• Main action button \"Place bid - [Amount value]\""
      },
      {
        "id": "AC-012-007",
        "given": "The user is on the product detail page at the bottom-right panel section.",
        "when": "The page finishes loading",
        "then": "The system provides a toggle activation option for \"Autonomous Bidding\" along with a \"Maximum bid limit\" input field and a \"Set Auto Bid\" confirmation button."
      },
      {
        "id": "AC-012-008",
        "given": "The user is actively viewing the product detail page for \"Vintage Rolex Submariner 1968\".",
        "when": "The system detects a network failure or WebSocket disconnection (Connection lost).",
        "then": "• The top navigation bar displays a red badge: \"Connection interrupted\".\r\n• A yellow warning banner appears below the header stating: \"Live prices may be outdated. Last confirmed [Timestamp]. Reconnecting... Do not assume a submitted bid has failed or succeeded.\" alongside a \"Retry now\" button.\r\n• The Live indicator status component on the right panel changes to a red badge: \"Connection interrupted\".\r\n• The \"My position\" component displays a red text status: \"Connection lost\".\r\n• A pink information alert banner is injected in the bidding panel block stating: \"Connection interrupted. Do not assume a submitted bid has failed or succeeded until its status is confirmed.\""
      },
      {
        "id": "AC-012-009",
        "given": "The network status warning banner and badges are visible due to a lost connection.",
        "when": "The user clicks the \"Retry now\" button on the yellow warning banner.",
        "then": "• The system attempts to re-initiate the WebSocket/API connection handshake.\r\n• If successful, the warning banners disappear, and data fields (Current highest bid, Time remaining, Bid history) refresh to their latest real-time states."
      }
    ]
  },
  {
    "id": "US-013",
    "title": "US-013 - Operations & Market Overview Dashboard",
    "description": "As a Bidder, Auction Manager, or Super Admin, I want to monitor the live bid stream and auction status in real time, so that Bidders can track market competition, AMs can oversee lot operations, and Super Admins can verify system integrity",
    "story_sizing": {
      "new_tech_score": 0,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 0,
      "tech_flow_weight": 30,
      "ac_score": 3,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 90,
      "size": "S",
      "apps_point": 2.5,
      "new_tech_total": 0,
      "surrounding_total": 15,
      "tech_flow_total": 0,
      "ac_total": 75,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-013-001",
        "given": "The user (Bidder, AM, or Super Admin) is logged into the Auction Engine platform and opens the Overview Dashboard",
        "when": "The system establishes an active WebSocket/live network connection.",
        "then": "• The top header status pill component must dynamically render a green indicator showing \"● Live • synced\".\r\n• All numerical metrics, countdown timers, and tables must update asynchronously without requiring a manual browser refresh."
      },
      {
        "id": "AC-013-002",
        "given": "The Bidder is viewing the top grid panel of the dashboard workspace.",
        "when": "The background live sync data framework receives new transaction events.",
        "then": "The system must instantly display and balance check four core tracking metrics:\r\n• Available Balance: Shows ready-to-use funds (e.g., Rp48.250.000) with estimated changes underneath.\r\n• Locked Collateral: Highlights funds currently frozen for active bids (e.g., Rp9.500.000).\r\n• Active Bids: Displays total active count (e.g., 8) with a red warning subtext if user is outbid (e.g., 3 outbid / 1 highest).\r\n• Won this month: Shows successful historical outcomes."
      },
      {
        "id": "AC-013-003",
        "given": "The user has items saved to their personal auction watchlist",
        "when": "An item in the \"Ending soon on your watchlist\" slider receives a new bid or reaches a critical time state.",
        "then": "• The card overlay badge updates dynamically based on status (e.g., changing to a yellow blinking \"🔥 Ending soon\" or yellow \"⏳ Extended\" text badge).\r\n• The \"Current bid\" value flashes and updates instantly.\r\n• The \"Time Remaining\" countdown clock decrements perfectly every second."
      },
      {
        "id": "AC-013-004",
        "given": "The user (especially Bidder tracking market or AM overseeing lots) scrolls to the \"My active bids\" / \"Lot Management\" grid table.",
        "when": "Any status permutation transition occurs on a live item lot.",
        "then": "The system row must change state instantly, applying the correct color-coded visual indicator badges under the Status column:\r\n• Leading (Green): User currently holds the valid highest bid.\r\n• Outbid (Red): Another participant has overtaken the price matrix.\r\n• Extended (Orange): Anti-snipe time rule extension rules have kicked in.\r\n• Active (Blue): Open for standard bidding interaction.\r\n• Scheduled (Grey): Future lot item not yet open for live bids."
      },
      {
        "id": "AC-013-005",
        "given": "The Super Admin, AM, or Bidder looks at the \"Recent activity\" timeline list at the bottom container area.",
        "when": "Any transaction, system automated action, or network anomaly event is broadcasted.",
        "then": "The system must immediately append chronological system event lines with specific descriptive logging badges:\r\n• Bid Success (Green check): \"Your bid Rp12.500.000 was accepted\".\r\n• Outbid Notice (Red caution): \"You have been outbid\".\r\n• Auto-Bid Automation (Purple lightning): \"Auto-bid executed Rp12.400.000\".\r\n• Time Extension Rule (Yellow clock): \"Auction extended by 2 minutes\"."
      },
      {
        "id": "AC-013-006",
        "given": "The system ledger detects that an active logged-in user has won an item but has not settled the invoice block.",
        "when": "The Overview dashboard page finishes loading.",
        "then": "• A high-priority blue systemic notice banner appears at the very top workspace grid section stating: \"Payment due for 1 won auction. Settle by [Date/Timestamp]. Balance must clear...\".\r\n• A functional \"Pay now\" action button is displayed on the far right edge of the alert element box to bypass and speed up the checkout settlement routing."
      }
    ]
  },
  {
    "id": "US-014",
    "title": "US-014 - View Lot Management List & Catalog Lifecycle (AM Exclusive)",
    "description": "User Story – View Lot Management List\r\n\r\nAs an AM, I want to view the list of lots in Lot Management, so that I can review and manage lot information according to my responsibilities.",
    "story_sizing": {
      "new_tech_score": 0,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 0,
      "tech_flow_weight": 30,
      "ac_score": 2,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 65,
      "size": "S",
      "apps_point": 2.5,
      "new_tech_total": 0,
      "surrounding_total": 15,
      "tech_flow_total": 0,
      "ac_total": 50,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-014-001",
        "given": "The Auction Manager (AM) is logged into the Admin Workspace",
        "when": "The AM clicks on the \"Lot Management\" menu link option in the persistent sidebar.",
        "then": "• The system displays the main \"Lot Management\" workspace interface.\r\n• The top header status pill component must dynamically show a green indicator \"● Live • synced\".\r\n• The system displays a top contextual banner describing the catalog lifecycle: \"Catalog of all lots. Draft → Scheduled → Active → Closed → Archived. Published lots lock critical fields.\""
      },
      {
        "id": "AC-014-002",
        "given": "The AM is viewing the upper control section of the Lot Management workspace.",
        "when": "The page finishes loading.",
        "then": "The system must render five lifecycle tab filters complete with active counters:\r\n• All (e.g., 312) \r\n• Draft (e.g., 14) \r\n• Scheduled (e.g., 18) \r\n• Active (e.g., 24) \r\n• Closed (e.g., 198) \r\n• Archived (e.g., 58)"
      },
      {
        "id": "AC-014-003",
        "given": "The AM wants to narrow down or rearrange the displayed lot records list grid.",
        "when": "The AM interacts with the filter drop-down components or the sort selector toggle.",
        "then": "• The UI provides drop-down components for Category (e.g., All), Type (e.g., All), Status, Price range, and Ending parameter metrics.\r\n• The system provides a \"Sort: Ending soon\" dropdown toggle list layout to control chronological row ordering layouts."
      },
      {
        "id": "AC-014-004",
        "given": "The AM is reviewing the primary catalog listing table view",
        "when": "The list records are fetched and populated.",
        "then": "The grid table must render the following specific structural data columns for each row:\r\n• Selection Checkbox: An interactive square component on the far left column margin.\r\n• Lot (Identity): Displays the item name asset title and the subtext unique Lot alphanumeric identifier code (e.g., \"Rolex GMT-Master 1675\", \"LOT-2026-0161\").\r\n• Category: Renders the mapped asset grouping (e.g., Watches, Art, Cameras, Bags, Vehicles).\r\n• Type: Renders the auction mechanic format (e.g., Bounded Auction, Classic Auction).\r\n• Start Price: Displays standard formatted rupiah prices (e.g., Rp12.000.000).\r\n• Schedule: Renders planned duration windows or historical closed timelines (e.g., 31 Oct 09:00-14:00, Not scheduled, 20 Oct (ended))."
      },
      {
        "id": "AC-014-005",
        "given": "The AM is monitoring the active list rows under the Status column matrix.",
        "when": "A lot transitions across structural operational parameters.",
        "then": "The system row must change badge layout styling and text dynamically:\r\n• Active (Blue icon & text): Displays for items currently receiving live bids.\r\n• Scheduled (Grey folder icon): For finalized items awaiting launch windows.\r\n• Draft (Grey pencil icon): For incomplete items awaiting setup completion.\r\n• Ending soon (Orange clock badge): Critical time window phase warning status.\r\n• Extended (Orange clock badge): Triggered when anti-snipe buffer time kicks in.\r\n• Closed (Grey padlock icon): Triggered when the expiration timeline window concludes."
      },
      {
        "id": "AC-014-006",
        "given": "The AM looks at the far right column component margin of a specific item lot row.",
        "when": "The AM views the dynamic context link indicator or clicks the element tool trigger.",
        "then": "The system renders an operation text link mapped strictly to the item's live status state:\r\n• If status is Active, Ending soon, Extended, or Closed, display a \"View\" link to monitor live activities.\r\n• If status is Scheduled or Draft, display an \"Edit\" link to modify parameters.\r\n• If status is Closed (and meets retention rules), display an \"Archive\" link to shift the lot to storage records."
      },
      {
        "id": "AC-014-007",
        "given": "The catalog data records exceed the standard 8-row viewport page listing threshold interface.",
        "when": "The AM interacts with the pagination buttons at the bottom right corner",
        "then": "• The system renders a tracker stating current index metrics (e.g., \"Showing 1-8 of 312 lots\").\r\n• Interactive navigation buttons labeled \"Previous\" and \"Next\" allow switching between table pages."
      },
      {
        "id": "AC-014-008",
        "given": "The AM is working actively anywhere inside the Lot Management table panel.",
        "when": "A global transaction milestone occurs on the platform (e.g., a bidder's offer is processed by the core ledger).",
        "then": "• The system slides out a real-time toast alert box at the top right workspace corner: \"✓ Bid accepted\".\r\n• The body layout of the toast details specific audit timestamps (e.g., \"Your bid of Rp12.600.000 was accepted at 10:04:31.\") with an interactive close (X) box icon."
      }
    ]
  },
  {
    "id": "US-015",
    "title": "US-015 - AM Live Auction Monitor & Authorized Interventions",
    "description": "As a AM I want to monitor the live bid stream and auction status in real time, so that Bidders can track market competition, AMs can oversee lot operations, and Super Admins can verify system integrity",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 2,
      "tech_flow_weight": 30,
      "ac_score": 2,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 140,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 15,
      "tech_flow_total": 60,
      "ac_total": 50,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-015-001",
        "given": "The Auction Manager (AM) is logged into the Admin Workspace.",
        "when": "The AM clicks on the \"Live Monitor\" menu link option in the persistent sidebar.",
        "then": "• The system displays the \"Live auction monitor\" workspace layout.\r\n• The header sub-text must state: \"Real-time view. Interventions require a reason and are written to the audit log. Bid outcomes cannot be edited.\"\r\n• The system status indicator must show a green badge \"● Live - synced 10:04:31\".\r\n• All widgets, streams, and graphs must process async data smoothly without requiring a manual window refresh.",
        "technical_flow": "FE: Client initializes EventSource connection to /api/v1/sse/auctions/:id"
      },
      {
        "id": "AC-015-002",
        "given": "The AM is reviewing the list of ongoing lots on the left-hand panel titled \"Active auctions (24)\".",
        "when": "The AM clicks on a specific auction card item (e.g., \"Hermès Birkin 30 Togo\").",
        "then": "• The selected auction item card highlights with a dark bold border outline.\r\n• The main workspace panel layout (Metrics, Price History, Bid Stream, and Interventions) instantly shifts context to render the real-time data of that selected lot.",
        "technical_flow": "BE SSE Hub: Establish persistent HTTP/2 chunked response stream (text/event-stream)"
      },
      {
        "id": "AC-015-003",
        "given": "The AM is tracking the live operational metrics grid at the top center of the dashboard.",
        "when": "New real-time network packets and bids pass through the socket connection layer.",
        "then": "The system must instantly display and balance check three core live metrics counters:\r\n• Bids / 5 min: Shows incoming bid volume acceleration speed (e.g., 31 bids with indicator +24% vs 5 mins ago).\r\n• Unique bidders: Displays unique consumer participants (e.g., 19 active with subtext 4 using auto-bid).\r\n• Gateway p95: Monitors data latency platform response time (e.g., 820ms with threshold reference Target < 500ms).",
        "technical_flow": "BE Engine: High-concurrency serialization in Redis Lua (< 5 ms execution)"
      },
      {
        "id": "AC-015-004",
        "given": "The AM looks at the top right section of the active lot monitor dashboard workspace.",
        "when": "The auction server clock ticks down or a bidder submits an accepted price increase.",
        "then": "• Current valuation: System prints large typography text \"Rp214.000.000\" next to a green \"▲ 34\" total bids milestone badge counter.\r\n• Time remaining: A yellow alarm banner box displays a real-time countdown timer tracking hours, minutes, and seconds (00:06:32) complete with anti-snipe buffer metrics subtext (\"Extended +0:02 - ends at 10:08:41\").",
        "technical_flow": "BE SSE Hub: Multiplex & dispatch price mutation frames to all active observers simultaneously"
      },
      {
        "id": "AC-015-005",
        "given": "The AM monitors the \"Bid event stream\" list feed in the center workspace.",
        "when": "Transactional webhooks or network connectivity state variations occur on the system.",
        "then": "The system must instantly append chronological updates complete with descriptive event color icon indicators:\r\n• Manual Bid (Blue hammer): \"Bidder AA-2201 bid Rp12.500.000\".\r\n• System Extension (Yellow clock): \"Auction extended by 2 minutes - Anti-snipe rule\".\r\n• Auto-Bid Action (Purple lightning): \"Auto-bid placed Rp12.400.000 - Max proxy Rp15.000.000\".\r\n• Network Status (Grey warning icon): \"Connection restored - data resynced\"",
        "technical_flow": "Prometheus Exporter: Scrape telemetry & confirm latency SLA < 300 ms under 10k req/s load"
      },
      {
        "id": "AC-015-006",
        "given": "The AM reviews the \"System health\" tracking sidebar panel on the right margin.",
        "when": "The infrastructure manager framework tracks concurrent users and drop statistics.",
        "then": "The table must continuously print diagnostic metadata parameters containing:\r\n• WebSocket channels: Total active connected clients (e.g., 1,254 connected).\r\n• Bid gateway p95: Instant latency tracker (e.g., 820ms).\r\n• Event log: Throughput frequency speed tracking metrics (e.g., 120ms).\r\n• Packet loss (%): Operational drop safety checker indicator (e.g., 0)."
      },
      {
        "id": "AC-015-007",
        "given": "The AM is reviewing an active auction experiencing anomalous patterns or urgent issues.",
        "when": "The AM triggers an entry point button inside the \"Authorized interventions\" section at the bottom center.",
        "then": "The system exposes four operational mitigation action buttons:\r\n• \"⏸ Pause 5 min\": Halts incoming bids and freezes timers temporarily.\r\n• \"🏳 Flag bidder\": Opens an interface to penalize a bad actor.\r\n• \"💬 Notify bidders\": Broadcasts a system banner message alert layout.\r\n• \"🛑 Cancel auction...\": A red primary button that opens a confirmation window requiring an explicit reason text log input to permanently terminate the active lot event."
      }
    ]
  },
  {
    "id": "US-016",
    "title": "US-016 - Participants Directory & KYC Compliance Ledger (AM Exclusive)",
    "description": "As a AM I want to monitor the live bid stream and auction status in real time, so that Bidders can track market competition, AMs can oversee lot operations, and Super Admins can verify system integrity",
    "story_sizing": {
      "new_tech_score": 1,
      "new_tech_weight": 15,
      "surrounding_score": 1,
      "surrounding_weight": 15,
      "tech_flow_score": 2,
      "tech_flow_weight": 30,
      "ac_score": 2,
      "ac_weight": 25,
      "ui_slicing_score": 0,
      "ui_slicing_weight": 15,
      "total_score": 140,
      "size": "M",
      "apps_point": 2.5,
      "new_tech_total": 15,
      "surrounding_total": 15,
      "tech_flow_total": 60,
      "ac_total": 50,
      "ui_slicing_total": 0
    },
    "acceptance_criteria": [
      {
        "id": "AC-016-001",
        "given": "The Auction Manager (AM) is logged into the Admin Workspace and opens the \"Participants\" view panel page.",
        "when": "The page finishes loading.",
        "then": "The system must instantly render four core analytical stats tracking counters:\r\n• Verified bidders: Displays total count (e.g., 3.412) with weekly speed growth indicators (▲ +85 this week).\r\n• Pending verification: Tracks items awaiting KYC process checks (e.g., 27 with subtext Avg. 4h to review).\r\n• With active collateral: Tracks locked funds currently frozen for active bids (e.g., 1.108 participants with total value metadata Rp4.2 B total).\r\n• Restricted: Highlights account blocks (e.g., 6 with an urgent red notification badge ⚠ 2 new today).",
        "technical_flow": "FE: Client initializes EventSource connection to /api/v1/sse/auctions/:id"
      },
      {
        "id": "AC-016-002",
        "given": "The AM wants to target a specific subset segment array of the customer directory ledger.",
        "when": "The AM selects items inside the filter drop-down menus (Category, Type, Status, Price range, Ending).",
        "then": "• The system generates removable active blue tag filter blocks under the header margin layout (e.g., Category: Watches ✕, Type: Bounded Auction ✕).\r\n• The central table row rows instantly clear and refresh asynchronously to isolate data records matching the active conditions grid.",
        "technical_flow": "BE SSE Hub: Establish persistent HTTP/2 chunked response stream (text/event-stream)"
      },
      {
        "id": "AC-016-003",
        "given": "The AM is on the persistent main directory table page dashboard space.",
        "when": "The AM clicks the \"Export\" functional action button link block located at the top-right container workspace border area.",
        "then": "• The system aggregates the active data grid entries array mapping (including Bidder ID, Verification status, Collateral value, Active bids, Won metrics, and Location data).\r\n• The system bundles the backend query dataset and dispatches a file takeout download package matching structural standard formatting criteria (.CSV or .XLSX).",
        "technical_flow": "BE Engine: High-concurrency serialization in Redis Lua (< 5 ms execution)"
      },
      {
        "id": "AC-016-004",
        "given": "The active database registers are successfully fetched and pulled to the dashboard view viewport.",
        "when": "The page layout rendering process initiates.",
        "then": "The directory table list grid layout template container object container components must print exactly six systematic operational meta data columns:\r\n• Bidder (Identity): Displays Customer Full Name, unique alphanumeric hashtag hash tracker parameter index tag, and geographic residential base city location metrics (e.g., \"Ayya Adzani\", \"#B-1042\", \"Jakarta\").\r\n• Verification: Prints the structural compliance state identifier validation stamp badge row configuration.\r\n• Collateral: Formats current frozen escrow financial assets balance (e.g., Rp25.000.000 / -).\r\n• Active Bids: Displays active concurrent transaction ticket counters (e.g., 14).\r\n• Won /90th: Total items awarded successfully over a rolling 90-day time scope ledger interval window.\r\n• Last Active: Displays time lapse updates relative to system server timestamps (e.g., Now, 2 min ago, 1 h ago, 12 Oct).",
        "technical_flow": "BE SSE Hub: Multiplex & dispatch price mutation frames to all active observers simultaneously"
      },
      {
        "id": "AC-016-005",
        "given": "The AM is inspecting the active database list grid table layout view components",
        "when": "The background compliance validation layer switches account flags or user updates documentation files.",
        "then": "The system column records container must instantly re-style visual badge assets to conform to specific compliance micro-states:\r\n• Verified (Green check): Confirms verified authentication checks status clear credentials.\r\n• Under review (Red caution): Warns about security risk or pseudonymized accounts awaiting auditing (e.g., \"Bidder #A-2291\").\r\n• Pending (Orange clock): New onboarding KYC records queue state pipeline target link.\r\n• Restricted (Grey padlock): Accounts penalized under risk mitigation compliance criteria guidelines rules.",
        "technical_flow": "Prometheus Exporter: Scrape telemetry & confirm latency SLA < 300 ms under 10k req/s load"
      },
      {
        "id": "AC-016-006",
        "given": "The AM looks at the operational action command button alignment block on the extreme right border margin area of an item row.",
        "when": "The user table row renders on screen.",
        "then": "The component maps and renders specific action control buttons mapped strictly to the target participant's security verification state:\r\n• If verification is Verified or Restricted, show a subtle text link option button labeled \"View\" to check account sheets.\r\n• If verification is Under review, display an active button component labeled \"Review\" to force inspection parameters.\r\n• If verification is Pending, display a primary dark-blue actionable button block component labeled \"Verify\" to invoke standard credentials authorization."
      }
    ]
  }
]
```

---

## 3. Rincian Naratif User Story & Acceptance Criteria

### 1. [US-AUTH] US-AUTH - Multi-Role Authentication with Progressive Lockout

> **Story:** As a system user (Bidder, Auction Manager, or Super Admin), I want to log in using the authentication method that matches my role (NIRO for Bidder and Auction Manager, Email and Password for Super Admin) so that I can securely access the workspace for my role, and the system rejects any access that does not comply with the rules.

**Sizing Appraisal:** Size **M** (Total Score: **155** | Tech Flow: 60 | AC Score: 50)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | A Bidder user is on the Sign in page | The user clicks 'Login with NIRO' and successfully authenticates | The system issues a JWT session with role 'BIDDER' and redirects the user to the Bidder Workspace | `Login Flow FE -> BE: Authorize URL Redirect BE -> FE: Return Login URL FE: Redirect and Login to Receive Niro Token FE -> BE: Auth Login Token` |
| **2** | An Auction Manager user is on the Sign in page | The user clicks 'Login with NIRO' and successfully authenticates | The system recognizes their promoted 'ADMIN' role and redirects the user to the Auction Manager Workspace (not the Bidder workspace) | `BE -> NIRO: Validate and Get User Info based Niro Login Token` |
| **3** | A Bidder or Auction Manager user is on the Sign in page | The user attempts to submit the Email & Password form | The system rejects the login attempt, does not create a session, and displays: 'Please use Login with NIRO for your account type | `Cek Registration Status == IF Registered BE: Get User Info` |
| **4** | A Super Admin user is on the Sign in page | The user inputs a valid Email and Password (min. 12 characters) and submits | The system issues a JWT session with role 'SUPERADMIN', resets any failed login counters, and redirects to the Super Admin Portal | `== IF Not Registered BE: Insert User Info & Insert Wallet Info` |
| **5** | A Super Admin user is on the Sign in page | The user enters an incorrect password 3 consecutive times | The system locks the Super Admin login for 1 minute (60 seconds) and displays HTTP 429 with retry-after information | `BE -> FE: Return Login with Token` |
| **6** | A Super Admin login has already been locked for 1 minute once | The user fails another 3 consecutive login attempts immediately after the first lock expires | The system doubles the lockout duration to 2 minutes (120 seconds) | `-` |
| **7** | A Super Admin login has already been locked twice (1 minute and 2 minutes) | The user fails another 3 consecutive login attempts | The user fails another 3 consecutive login attempts | `-` |
| **8** | A Super Admin is logged in to the Super Admin Portal | The Super Admin selects an existing NIRO user and assigns the role 'ADMIN' | The user's role is updated in the database, granting them immediate access to the Auction Manager Console on subsequent logins | `-` |

---

### 2. [US-001] US-001 - Top Up E-Wallet Balance (Bidder Exclusive & Step-by-Step UI)

> **Story:** As a Bidder, I want to top up my e-wallet balance with a minimum amount of IDR 100,000, so that I have sufficient available balance to participate in and place bids on luxury auctions

**Sizing Appraisal:** Size **M** (Total Score: **115** | Tech Flow: 60 | AC Score: 25)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | The Bidder is on Step 1 (Amount) of the top-up journey. | The Bidder manually types an amount less than the minimum required threshold (e.g., 25.000). | • The system blocks the progress workflow. • The input field displays a red border wrapper outline. • A red inline validation text error message is displayed directly underneath the input box: "Minimum top up is IDR 100,000". • The primary "Continue" button remains entirely disabled. | `FE -> BE: GET /wallet/me BE -> DB/Redis: Fetch liquid available_balance and locked_balance (float64)` |
| **2** | The Bidder is on Step 1 (Amount) of the top-up journey. | The Bidder inputs a non-standard currency value or fractions that are not whole rupiah multiples (e.g., 75.000,004). | • The system triggers real-time data inspection. • The input field border changes to a distinct red warning state color. • A red system message box appears stating: "Minimum IDR 100.000 / per top-up, whole rupiah only.". • An explanatory contextual warning block details: "Note: our platform reserves 10% of the bid from your Available balance, so Rp1.000.000 bid will lock up to Rp1.100.000 from your account." | `FE -> BE: POST /wallet/topup {amount >= 100000} BE: Validate role and Amount` |
| **3** | The Bidder has successfully passed Step 1 with a valid value and has transitioned into the Step 2 (Method) configuration view. | The Bidder attempts to click the primary action button "Continue to payment" before explicitly checking any Virtual Account or E-Wallet radio component cards. | • The system halts submission routing execution logic. • A high-priority red alert warning card notification panel appears at the top center of the main body workspace container stating: "Choose a payment method". • Focus is dynamically directed toward the payment channels selection field grid. | `BE: INSERT INTO ledger_entries (type: WALLET_TOPUP)` |
| **4** | The user is authenticated and logged in as an Auction Manager or Super Admin. | The user attempts to hit the top-up endpoint or access the /profile/wallet/topup page layout | • The system denies routing access. • The API layer rejects the transaction with an access error message: "Wallet top-up is exclusive to Bidder accounts." | `BE -> FE: Return 200 OK with updated balance envelope` |
| **5** | The Bidder is on Step 3a (Pay) and has initiated a Virtual Account or E-wallet checkout flow. | The system payment gateway processing core encounters an unhandled system exception or returns a failure status. | • A high-visibility red error alert notification card appears at the top center stating: "Payment processor failed". • An inline sub-text message appears instructing the user to double-check the billing parameters or switch to alternative payment channels. • The primary action button resets to a retryable active state. | `FE: Update Wallet header chips & trigger success toast notification` |
| **6** | The Bidder is on Step 3c (OVO Push Notification validation or security token verification view). | The Bidder inputs an incorrect or expired verification token security PIN sequence. | • The input field displays a bright red structural error outline color. • A red inline alert banner is dynamically injected stating: "Invalid authentication code". • The system locks validation processing execution until a fresh secure token is requested or re-entered. | `-` |
| **7** | The Bidder is on any Step 3 payment processing layout with a countdown clock component. | The countdown timer reaches the zero threshold boundary (00:00:00) before transaction verification occurs. | • A yellow warning alert notice box appears declaring: "Payment session expired". • The interface restricts direct payment submission execution logic. • A primary utility option button labeled "Restart transaction" or "Re-create top up" becomes available to safely rebuild the invoice parameters. | `-` |
| **8** | The Bidder is waiting on the Step 3e checking/processing screen layout. | The active secure network socket handshake encounters an unexpected connection breakdown state. | • A high-priority yellow caution banner pops up inside the workspace declaring: "Connection lost. Re-establishing link...". • The submission status handler disables actions to strictly block users from submitting duplicate requests. • A dynamic loader attempts to re-verify status seamlessly without dropping user transaction state history data. | `-` |
| **9** | The Bidder attempts to finalize an automatic direct debit account settlement link pipeline channel. | The chosen institution engine reports a structural institutional ceiling check failure or account insufficiency response. | • A red system alert banner component is mounted to the view dashboard: "Account balance insufficient or payment channel limit exceeded.". • The specific bank card item profile updates into a critical system warning mode styling border layout. • The workflow redirects options back to alternative payment method interfaces. | `-` |

---

### 3. [US-002] US-002 - Dynamic Collateral Hold During Bidding

> **Story:** As a Bidder, I want my available balance to be validated and held as collateral upon submitting a bid, and to remain held throughout the entire auction period until settlement, so that all bids are backed by real funds

**Sizing Appraisal:** Size **M** (Total Score: **175** | Tech Flow: 90 | AC Score: 25)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | Bidder has available balance of IDR 100,000 and locked balance of IDR 0 | Bidder places a bid of IDR 75,000 | Bid is accepted, IDR 75,000 is moved to locked balance, and available balance becomes IDR 25,000 | `FE -> BE: Submit Increment Bid Action BE -> Redis: Verify available_balance >= target_bid` |
| **2** | Bidder has an active held bid of IDR 75,000 | Another participant submits a higher bid of IDR 85,000 (outbid event) | The initial bidder's IDR 75,000 remains in locked balance and is NOT immediately released during the ongoing auction period | `Redis: Atomically deduct available_balance & increase locked_balance` |
| **3** | Bidder has available balance of IDR 25,000 | Bidder attempts to click a bid option requiring IDR 50,000 | The button is disabled with an explanatory message: 'Insufficient available balance. Please top up | `== IF Outbid Event Occurs: Redis: Update auction highest_bidder_id Redis: Retain previous bidder held funds in locked_balance (NO release during active auction)` |
| **4** | The auction concludes and the Bidder did not win | The settlement daemon processes finality | The Bidder's held IDR 75,000 is released 100% back to available balance | `== IF Auction Concludes: BE Settlement: Permanently debit winner locked_balance BE Settlement: Release 100% held funds for all losing participants to available_balance` |

---

### 4. [US-003] US-003 - Place Incremental Bid on an Uncapped Window Auction

> **Story:** As a Bidder, I want to place manual bids using pre-configured increment buttons on high-value asset categories without typing free text, competing within a time window defined by the Auction Manager (e.g. 1 day).

**Sizing Appraisal:** Size **M** (Total Score: **170** | Tech Flow: 90 | AC Score: 50)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | A user is logged in as a Bidder, Viewing a high-value asset detail page with a Current Price of IDR 350,000,000, And the allowed pre-configured increments are +IDR 5m and +IDR 10m | The Bidder clicks the +IDR 10m button | • The New Price automatically updates to IDR 360,000,000 without any free text typing. • the Bidder becomes the Highest Bidder. • The price updates in real-time for all observers in < 300 ms. | `FE: Bidder clicks pre-configured discrete increment button (+Rp 5m / +Rp 10m)` |
| **2** | A user is logged in as a Bidder and has clicked an increment button | The system triggers a Bid Confirmation Pop-up showing the exact bid summary | • The primary action button on the pop-up processes the official bid submission. • A close or cancel option is provided to dismiss the action without placing a bid. • The modal must display the correct Asset Title ("Vintage Rolex Submariner 1968"). • It must clearly show the Bid Amount being placed (IDR 360,000,000). • It must list the Bidder's Virtual Account / Wallet Info for verification. | `FE -> BE: POST /api/v1/auctions/:id/bids` |
| **3** | The "Confirm your bid" Pop-up modal is actively displayed on the Bidder's screen | The Bidder clicks the "Confirm Bid" button (primary blue button) | • The pop-up closes immediately. • The system processes the bid submission. • The main screen background updates smoothly to reflect the new highest bidder status. | `BE: Validate increment matches allowed_increment_steps (Reject free text input)` |
| **4** | The "Confirm your bid" Pop-up modal is actively displayed on the Bidder's screen | The Bidder clicks the "Cancel" / "Back" button or the "X" icon | • The pop-up closes immediately. • No bid is submitted to the system. • The main auction page remains in its exact previous state with no changes to the current price. | `BE -> Redis: EVALSHA bid_placement.lua Redis: Check wallet available_balance >= new_price Redis: Atomic mutation of current_price & highest_bidder_id` |
| **5** | The "Confirm your bid" Pop-up modal is actively displayed on the screen | The live auction countdown timer is ticking down in the background | • The remaining time window (e.g., 00:04:32) must remain visible or sync correctly inside/behind the modal. • If the time window expires (00:00:00) while the pop-up is still open, the modal must automatically close and show a "Time Expired" alert to prevent late submission. | `Redis -> PubSub: Publish internal price_updated event` |
| **6** | Two distinct Bidders are viewing the same active auction page | Both Bidders click the increment button simultanesouly | • The system processes both incoming requests via Redis Lua atomic serialization. • The first arriving bid is accepted. • The second arriving bid is immediately evaluated against the newly incremented price with zero race condition or duplicate data errors. | `BE SSE Hub: Broadcast PRICE_UPDATED payload to all connected clients in < 300 ms` |
| **7** | A user is logged in as an Auction Manager or Super Admin | Viewing the auction detail page | • The manual bid action buttons (Quick Increment Buttons & Place Bid Column) are completely hidden or disabled. • This strictly prevents any internal conflict of interest during the live auction. | `BE -> DB: Asynchronously append bid log into bids table` |
| **8** | A user is logged in as a Bidder on the auction detail page | Observing the remaining time on the Countdown Timer component | • Bidders can only compete within the specific time window defined by the Auction Manager (e.g., 1 day). • Once the timer hits 00:00:00, all bidding actions are automatically locked out in real-time. | `-` |

---

### 5. [US-004] US-004 - Velocity-Based Anti-Sniping Dynamic Timer Extension

> **Story:** As a Bidder and Auction Manager, I want the auction timer to automatically extend if high bidding velocity (>= 10 hits/second) is detected in the final 2 minutes, using the extension duration configured for that lot

**Sizing Appraisal:** Size **M** (Total Score: **115** | Tech Flow: 60 | AC Score: 25)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | Auction remaining time is 45 seconds (within the 2-minute critical zone) | Bid velocity reaches 10 bids in a single second and lot extension is set to 5 minutes | System extends the countdown by exactly 5 minutes and broadcasts the new end time via WebSocket | `BE Redis Lua: Check remaining_time <= 120 seconds (Last 2-Minute Critical Zone)` |
| **2** | Auction remaining time is 45 seconds | Bid velocity is only 4 bids in that second | System accepts the bids normally without extending the timer | `BE Redis: Calculate bid velocity count in rolling 1-second sliding window` |
| **3** | Auction remaining time is 5 minutes (outside the 2-minute critical zone) | Bid velocity reaches 15 bids in a single second | System accepts the bids normally without triggering anti-sniping extension | `== IF Velocity >= 10 bids/second: Redis: Trigger Anti-Sniping extension Redis: end_time = end_time + (extension_minutes * 60) Redis: Update ZSET score in auctions:expiry_zset` |

---

### 6. [US-005] US-005 - Instant Buy-Out on a Capped Value Auction

> **Story:** As a Bidder, I want to immediately purchase a luxury asset in a Capped Value auction by selecting the Buy-Out option or meeting the valuation ceiling, so that the auction concludes immediately without waiting for the window to expire.

**Sizing Appraisal:** Size **M** (Total Score: **115** | Tech Flow: 60 | AC Score: 25)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | A Capped auction is active with a valuation ceiling of IDR 500,000,000, and the Bidder's available balance is IDR 550,000,000. | The Bidder clicks the 'Instant Buy-Out' button. | • The auction closes immediately. • The system updates the status to 'CLOSED_AWARDED'. • The Bidder is declared the winner. • All active observers receive a live AUCTION_BUYOUT_CLOSED system event. | `FE: Bidder clicks "Instant Buy-Out" action button on Capped Valuation lot` |
| **2** | A Capped auction is active with a valuation ceiling of IDR 500,000,000, and the Bidder's available balance is only IDR 400,000,000 | The Bidder views the auction item detail page. | • The 'Instant Buy-Out' button is disabled/greyed out. • The system displays a tooltip or inline message: 'Insufficient balance for Buy-Out'. | `FE -> BE: POST /api/v1/auctions/:id/bids {buyout: true, amount: valuation_ceiling}` |
| **3** | The user is actively viewing the product detail page for "Vintage Rolex Submariner 1968" where they previously held the highest bid. | Another bidder places a higher bid that does not reach the ceiling value yet. | • The system instantly displays a red alert banner at the top right: "You have been outbid - Current highest bid: [Amount]. Review your bid." • The system updates the 'My Position' badge to a red "Outbid" status. • The main status badge next to the product title changes to "Outbid" to prompt immediate re-bidding or buy-out consideration. | `BE -> Redis: EVALSHA bid_placement.lua Redis: Validate available_balance >= valuation_ceiling Redis: Mutate lot status from ACTIVE to CLOSED_AWARDED immediately` |

---

### 7. [US-006] US-006 - Automated Proxy Bidding

> **Story:** As a Bidder, I want to set an automatic maximum proxy bid limit, so that the system raises bids incrementally on my behalf whenever I am outbid, up to my personal limit

**Sizing Appraisal:** Size **M** (Total Score: **130** | Tech Flow: 90 | AC Score: 25)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | A user is logged in as a Bidder with an available wallet balance ≥ IDR 450,000,000, And enters a Maximum bid limit of IDR 450,000,000 in the Autonomous Bidding card | The Bidder toggles the switch to active and clicks the "Start" button | • The system locks/validates the maximum proxy threshold. • The card status changes to active/running with visual checklist anchors (e.g., "Bids only when you are outbid", "Stops as your limit"). • The "Start" button turns into a red "Stop" button alongside a "Pause" option as shown in the Figma UI. | `FE -> BE: POST /api/v1/auctions/:id/proxy-bid {max_limit: float64} BE: Validate available_balance >= max_limit BE: Store proxy rule in PostgreSQL & Redis hash` |
| **2** | The Bidder's proxy bid is active with a limit of IDR 450,000,000, And the current minimum increment is IDR 5,000,000 | A competitor raises the asset price to IDR 390,000,000 | • The system proxy worker instantly calculates the next valid increment. • The system automatically places a counter-bid of IDR 395,000,000 on behalf of the proxy bidder in real-time. • The proxy bidder retains the "Highest Bidder" status. | `BE Proxy Worker: Subscribe to Redis PRICE_UPDATED event stream` |
| **3** | The Bidder's proxy bid is active with a limit of IDR 450,000,000 | A competitor places a higher bid of IDR 460,000,000 | • The system proxy worker immediately stops bidding. • The autonomous bidding state is marked as exhausted / inactive. • The Bidder instantly receives a push notification/alert stating they have been outbid. | `== IF Outbid Detected AND current_price + min_step <= max_limit: Proxy Worker: Calculate next increment Proxy Worker: Submit automated counter-bid on behalf of user` |
| **4** | The Autonomous Bidding worker is actively running in the background | The Bidder clicks the "Pause" or "Stop" button on the UI card | • The system immediately halts the automated agent loop. • The interface state toggles back to editable inputs. • No further automated bids are placed unless the user re-triggers the "Start" flow | `== IF current_price + min_step > max_limit: Proxy Worker: Mark proxy as EXHAUSTED Proxy Worker -> Notification: Dispatch Outbid push notification` |

---

### 8. [US-007] US-007 - Auction Lot Creation, Rule Configuration & Form Layout (AM Exclusive)

> **Story:** As an Auction Manager, I want to create luxury asset auction listings categorized by asset type, specifying minimum price, allowed increment steps, optional maximum buyout ceiling, window duration (e.g. 1 day), and optional anti-sniping extension minutes.

**Sizing Appraisal:** Size **M** (Total Score: **115** | Tech Flow: 60 | AC Score: 25)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | An Auction Manager fills all required fields with valid parameters without a ceiling price | AM clicks 'Publish Lot | Auction is saved as an 'UNCAPPED' time-window lot and immediately appears in the active catalog for all participants | `FE: Auction Manager fills lot specs, categories, start/end date, allowed steps, and attaches image` |
| **2** | An Auction Manager configures a Capped auction | Ceiling price is set less than or equal to the starting price | System rejects with HTTP 400: 'Valuation ceiling must be strictly greater than starting price | `FE -> BE: POST /api/v1/admin/auctions (multipart/form-data)` |
| **3** | The Auction Manager (AM) is on the "Operations overview" main dashboard workspace. | The AM clicks the primary blue action button "+ Create lot" located at the top-right section of the header. | The system terminates the dashboard rendering state and redirects the browser workflow to the blank "Create lot & auction" listing creation form layout. | `BE: Authenticate role == ADMIN (Auction Manager)` |
| **4** | The Auction Manager (AM) is authenticated and viewing any administrative layout workspace page. | The AM clicks the "Lot Management" menu link option available on the left-hand persistent navigation drawer menu (sidebar). | The system routes the view and grants the user direct access to the operational management panel flow and listing creation environment. | `BE: Read binary image multipart stream and compress to BYTEA BLOB BE: Store image_mime_type and record to auctions table` |
| **5** | The AM is on the "Create lot & auction" workspace interface. | The AM enters the asset identity specifications. | • Lot Title: Text input handles alphanumeric names (e.g., "Rolex GMT-Master 1675 'Radial'"). • Category: Dropdown allows filtering and selecting specific asset types (e.g., "Watches"). • Description: Text area supports detailed asset metadata, conditions, and completeness context. | `BE -> Redis: Initialize auction:{id} hash and insert end_time into auctions:expiry_zset` |
| **6** | The AM modifies any input parameters in Form Section 1 (Asset Details). | The data fields change state or text is typed | • The system dynamically renders a "Bidder preview" component box on the right sidebar panel. • The preview updates the Title, Category tag, and description text in real-time to mimic how it will look on the live Marketplace layout. | `BE -> FE: Return 201 Created with Base64 compressed image preview` |
| **7** | The AM scrolls down to the "Auction type & pricing" panel block. | The AM sets the structural financial configurations for the lot. | • Auction Type: The system provides toggle options (e.g., Standard Auction vs Capped/Ceiling Auction). • Starting Price: Input field locks numbers and sets the baseline minimum valuation (e.g., Rp10.000.000). • Bid Increment: Input field restricts minimal validation steps (e.g., Rp10.000.000 text field). • Buy-Out Ceiling (Optional): If enabled via toggle, a maximum threshold input field appears to capture the buyout value. | `-` |
| **8** | The AM proceeds to the "Schedule & extension" panel block. | The AM sets the operational timing limits for the auction lifecycle | • Start & End Time: Input fields provide integrated date-time picker selectors. • Anti-Sniping Extension: A dropdown menu allows selecting anti-snipe buffer minutes (e.g., "Choose Time Extension" options list: 2 Minutes, 5 Minutes, 10 Minutes, 15 Minutes). | `-` |
| **9** | The AM proceeds to the "Eligibility & collateral" block at the bottom of the form. | The AM defines security gate parameters. | • Eligible Bidders: Dropdown menu controls baseline identity restrictions (e.g., "Verified bidders only"). • Collateral Hold: A numerical or percentage input defines the frozen balance threshold needed to join (e.g., "10% of standard bid value"). | `-` |
| **10** | The AM has populated fields in the creation workspace. | The AM attempts to click the "Cancel" action button or navigate away via the sidebar menus before saving. | • The system halts the routing execution. • A confirmation modal dialog wrapper is mounted to the UI dashboard warning the user about unsaved progress loss. • Confirming cancellation clears state; cancelling modal returns the user to the form with full text inputs intact. | `-` |
| **11** | The AM fills out all mandatory asterisk (*) fields with valid data values. | The AM clicks the primary header button "Review & publish". | • The system validates inputs and triggers the database transaction orchestration layer. • A success notification banner appears. • The system automatically indexes the lot into the public listing inventory and updates the "Operations Overview" macro counter stats. | `-` |

---

### 9. [US-008] US-008 - Universal Live Bid & Concurrency Monitoring Stream

> **Story:** As a Bidder I want to monitor the live bid stream and auction status in real time, so that Bidders can track market competition, AMs can oversee lot operations, and Super Admins can verify system integrity

**Sizing Appraisal:** Size **M** (Total Score: **140** | Tech Flow: 60 | AC Score: 50)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | The user (Bidder, AM, or Super Admin) is logged into the Auction Engine platform and opens the Overview Dashboard. | The system establishes an active WebSocket/live network connection. | • The top header status pill component must dynamically render a green indicator showing "● Live • synced". • All numerical metrics, countdown timers, and tables must update asynchronously without requiring a manual browser refresh. | `FE: Client initializes EventSource connection to /api/v1/sse/auctions/:id` |
| **2** | The Bidder is viewing the top grid panel of the dashboard workspace. | The background live sync data framework receives new transaction events. | The system must instantly display and balance check four core tracking metrics: • Available Balance: Shows ready-to-use funds (e.g., Rp48.250.000) with estimated changes underneath. • Locked Collateral: Highlights funds currently frozen for active bids (e.g., Rp9.500.000). • Active Bids: Displays total active count (e.g., 8) with a red warning subtext if user is outbid (e.g., 3 outbid / 1 highest). • Won this month: Shows successful historical outcomes. | `BE SSE Hub: Establish persistent HTTP/2 chunked response stream (text/event-stream)` |
| **3** | The user has items saved to their personal auction watchlist. | An item in the "Ending soon on your watchlist" slider receives a new bid or reaches a critical time state. | • The card overlay badge updates dynamically based on status (e.g., changing to a yellow blinking "🔥 Ending soon" or yellow "⏳ Extended" text badge). • The "Current bid" value flashes and updates instantly. • The "Time Remaining" countdown clock decrements perfectly every second. | `BE Engine: High-concurrency serialization in Redis Lua (< 5 ms execution)` |
| **4** | The user (especially Bidder tracking market or AM overseeing lots) scrolls to the "My active bids" / "Lot Management" grid table. | Any status permutation transition occurs on a live item lot. | The system row must change state instantly, applying the correct color-coded visual indicator badges under the Status column: • Leading (Green): User currently holds the valid highest bid. • Outbid (Red): Another participant has overtaken the price matrix. • Extended (Orange): Anti-snipe time rule extension rules have kicked in. • Active (Blue): Open for standard bidding interaction. • Scheduled (Grey): Future lot item not yet open for live bids. | `BE SSE Hub: Multiplex & dispatch price mutation frames to all active observers simultaneously` |
| **5** | The Super Admin, AM, or Bidder looks at the "Recent activity" timeline list at the bottom container area | Any transaction, system automated action, or network anomaly event is broadcasted. | The system must immediately append chronological system event lines with specific descriptive logging badges: • Bid Success (Green check): "Your bid Rp12.500.000 was accepted". • Outbid Notice (Red caution): "You have been outbid". • Auto-Bid Automation (Purple lightning): "Auto-bid executed Rp12.400.000". • Time Extension Rule (Yellow clock): "Auction extended by 2 minutes". | `Prometheus Exporter: Scrape telemetry & confirm latency SLA < 300 ms under 10k req/s load` |
| **6** | The system ledger detects that an active logged-in user has won an item but has not settled the invoice block | The Overview dashboard page finishes loading | • A high-priority blue systemic notice banner appears at the very top workspace grid section stating: "Payment due for 1 won auction. Settle by [Date/Timestamp]. Balance must clear...". • A functional "Pay now" action button is displayed on the far right edge of the alert element box to bypass and speed up the checkout settlement routing. | `-` |

---

### 10. [US-009] US-009 - Automated Settlement & Reconciliation Queue (AM Exclusive)

> **Story:** As an Auction Manager, I want to oversee the automated settlement process when an auction ends, ensuring the winner's held funds are debited, losing participants' held funds are fully released, and asset ownership is legally recorded."

**Sizing Appraisal:** Size **M** (Total Score: **200** | Tech Flow: 120 | AC Score: 50)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | The Auction Manager (AM) is logged into the Admin Workspace | The AM clicks on the "Settlement Queue" menu link option in the persistent sidebar | • The system renders the "Settlement & reconciliation" dashboard tracking panel. • The header sub-text must state: "Auction result + settlement. Reconcile payments, handle exceptions, release collateral." • The system displays four real-time metric milestone cards: Pending payment (12), Processing (7), Exceptions (3), and Settled today (29). | `BE Daemon: 1-second interval poller runs ZRANGEBYSCORE auctions:expiry_zset -inf now()` |
| **2** | The AM is reviewing the upper section of the settlement transaction database list grid | The AM clicks on any category tab component filter link (e.g., All, Pending, Processing, Exceptions, Settled). | • The list grid dynamically switches rows to display only items matching that specific operational state query. • Each tab row accurately prints corresponding record total counters matching the master stats (e.g., clicking "Exceptions" displays exactly 3 records). | `BE Daemon: Acquire distributed lock SET lock:settle:{lot_id} NX EX 10` |
| **3** | The AM is viewing the active records grid inside the "Exceptions" filter tab menu container grid. | The AM clicks on a specific settlement invoice row item (e.g., "STL-2026-0079 - Leica M6"). | • The clicked item row highlights with an active background visual selection styling container framework. • The system dynamically mounts the granular invoice deep-dive breakdown summary inspector block panel on the right margin side area workspace. | `BE Daemon: Fetch final winner and highest bid amount from Redis` |
| **4** | The system loads the detailed panel view for an exception state transaction (e.g., STL-2026-0079). | The page finishes loading. | • The upper block displays a red structural error warning container component declaring: "Amount mismatch - Rp50.000 short". • The system prints the structural root cause audit log parameters: "Received Rp33.100.000 via BCA VA on 22 Oct, expected Rp33.150.000." | `BE -> DB: BEGIN TRANSACTION (SERIALIZABLE) DB: Debit winner locked_balance (insert AUCTION_WIN_PAYMENT) DB: Update auction status = SETTLED DB: Insert asset_ownership record DB: Release 100% held locked_balance for all losing bidders to available_balance DB: COMMIT TRANSACTION` |
| **5** | The AM is inspecting the visual tracking stepper component timeline inside the right-hand panel view workspace dashboard. | The backend background orchestrator triggers steps asynchronously. | The system updates the progress checkmark stepper layout color codes to map legal transitions: • ✓ Auction won (Green dot checkmark timestamp). • ✓ Collateral locked (Green dot checkmark displaying frozen funds e.g., Rp12.500.000). • ◌ Payment processing (Blue circle loader tracking gateway verification timelines e.g., Est. 1-2 business days). • ◌ Settled & ◌ Collateral released (Grey circles reflecting pending terminal milestones). | `BE -> Redis: Clean up active lot keys & ZREM from auctions:expiry_zset` |
| **6** | The AM determines a path to reconcile a disputed mismatch transaction state invoice. | The AM triggers an action button block inside the "Resolution" footer interface grid component. | The system processes three custom override logic sequences: • "Request Rp50.000": Dispatches an outstanding invoice balance reminder notification to the user profile link. • "Write off": Waives minor variations and pushes ledger forward. • "Mark reconciled": Manual blue CTA button that updates invoice status records to Settled and releases losing participants' collateral holds instantly. | `BE SSE Hub: Broadcast AUCTION_CONCLUDED event to all room participants` |

---

### 11. [US-010] US-010 - View Item Marketplace & Catalog Filtering

> **Story:** As a Bidder / Auction Participan I want to view the list of products currently up for auction So that I can select items that interest me and start placing bids

**Sizing Appraisal:** Size **M** (Total Score: **165** | Tech Flow: 60 | AC Score: 75)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | The user is on the "Marketplace" page | The system successfully loads the auction product data | The page displays product cards containing:  1. Category, 2. Item Name 3.  Lot ID, 4. Based Price 5. Latest Bid 6. Number of Bids 7. Countdown Timer 8. Watchlist Icon (Heart),  9. and a "View Detail" button. | `FE: User navigates to Marketplace page` |
| **2** | The user opens the "Marketplace" page | The system finishes loading the page | The system displays the total active auction products (e.g., "Showing 12 of 248 lots - Live updates on") along with the last updated time indicator. | `FE -> BE: GET /api/v1/auctions/active?page=1&limit=12&category=LUXURY_WATCHES` |
| **3** | The user is on the "Marketplace" page | The user clicks the "Type" filter dropdown (marked as F) and selects a specific type | The system filters and only displays products matching the selected type. | `BE: Query auctions table, calculate pagination metadata, decode BYTEA image to Base64` |
| **4** | The user is viewing the marketplace product list | An auction product is currently in progress or ongoing | The system displays the product card with an "Active" or "Extended" status label and shows the remaining time countdown. | `BE -> FE: Return standardized JSON envelope with items & pagination` |
| **5** | The user is viewing the marketplace product list | The user has placed a bid on a product and their bid amount is currently the highest | The system displays the product card with a "Leading" status label. | `FE: Connect to SSE /api/v1/sse/catalog to receive live price ticker updates` |
| **6** | The user is viewing the marketplace product list | An ongoing auction product is close to its time limit / almost finished | The system displays the product card with an "Ending Soon" status label. | `FE: Dynamic badge resolver (Active, Leading, Ending Soon, Extended, Scheduled)` |
| **7** | The user is viewing the marketplace product list | A product is registered for auction but the start time has not arrived yet | The system displays the product card with a "Scheduled" status label and shows the upcoming start date/time. | `-` |
| **8** | The user is on the first page of the marketplace | The user clicks the "Next" button at the bottom navigation | The system loads and displays the next set of products, and the text updates to "Page 2 of 21". | `-` |
| **9** | The user is on page 2 or higher of the marketplace | The user clicks the "Previous" button at the bottom navigation | The system loads and displays the previous set of products, and the page indicator decrements accordingly. | `-` |
| **10** | The user is on the first page ("Page 1 of 21") | The user views the pagination controls | The "Previous" button is disabled (unclickable) and styled visually as inactive. | `-` |
| **11** | There are no auction products at all in the system database | The user opens or navigates to the "Marketplace" page | The system displays an empty state page with an informative message: "There are currently no products up for auction" and hides the pagination controls completely. | `-` |
| **12** | The user is on the "Marketplace" page | The user enters a non-existent keyword or applies invalid filter combinations | The system displays a "no result found" message and hides the pagination controls since there are no pages to display. | `-` |

---

### 12. [US-011] US-011 - Participant Bid History & Settlement Tracking

> **Story:** As a Bidder, I want to view my bid history, so that I can review the details and status of my previous bids and track my bidding activity.

**Sizing Appraisal:** Size **M** (Total Score: **140** | Tech Flow: 60 | AC Score: 50)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | The user is on the "Bid History" page | The system successfully loads the data | The page displays summary widgets showing counts for: Active Bids, Leading, Outbid, and Won/Settled, along with descriptive sub-texts (e.g., "X values below reserve values"). | `FE: User navigates to Bid History page` |
| **2** | The user has an outstanding payment for a won auction | The user views the top section of the page | The system displays a yellow warning banner showing the payment deadline countdown (e.g., "Payment due in 1 day 22 hours") and total outstanding amount. | `FE -> BE: GET /api/v1/auctions/my-bids?status=ACTIVE` |
| **3** | The user is on the "Bid History" page | The user clicks on one of the activity tabs (All activity, Active Bids, Won, Lost) | The system filters and displays only the auction lots that match the selected status tab. | `BE: Aggregate active bids, leading lots, outbid counts, and won auctions` |
| **4** | The user is viewing the "Active Bidding" section | The system displays the active bidding table | The table must show detailed columns: Lot Info, My Bid, Highest Bid, Position Status (Leading/Outbid), Mode (Manual/Auto-Bid), Status (Countdown), and Action button (View/Bid open). | `BE -> FE: Return aggregated counts & lot list envelope` |
| **5** | The user scrolls down to the "Closed Auctions" section | The system populates the ended auction list | Each closed lot displays a visual step-by-step progress tracking bar with circles for: Won, Payment, Verification, and Collateral Released. | `FE: User clicks a specific closed auction item` |
| **6** | The user clicks or highlights a specific closed auction item | The side panel dynamically loads the data | The system displays the "Settlement detail" section on the right side, showing Hammer price, Buyer premium, Total cost, Collateral applied, Amount to pay, Due by date, and the chronological status steps (e.g., Auction won -> Collateral locked -> Payment processing). | `FE -> BE: Fetch settlement details (Hammer price, Buyer premium, Collateral applied)` |
| **7** | The user is viewing a settlement detail that requires payment | The user clicks the solid blue "Pay [Amount]" button at the bottom of the sidebar | The system redirects the user to the payment gateway or checkout page to settle the transaction. | `FE: Settle button redirects to payment/checkout if balance due exists` |
| **8** | The user has never participated in any auction or bidding activity | The user navigates to the "Bid History" page | The system displays an empty state illustration with an informative text: "You have no bidding history yet." and summary widgets show a count of 0. | `-` |
| **9** | The user applies multi-layered dropdown filters (Category, Type, Bid status, Price range) | The chosen filter combination does not match any historical data | The system displays an empty table layout with a message: "No bid history matches your active filters." | `-` |
| **10** | The user is actively monitoring countdowns on the table | The user's internet connection is lost | The system halts live counter updates and displays a toast alert message: "Connection lost. Retrying to sync your bid activities..." while keeping existing data on screen. | `-` |

---

### 13. [US-012] US-012 - Product Detail Page & Live SSE Room Integration

> **Story:** As a Bidder, I want to view product details, so that I can review the product information and evaluate the product before placing a bid.

**Sizing Appraisal:** Size **M** (Total Score: **110** | Tech Flow: 30 | AC Score: 50)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | The user is on the Marketplace page (Bidding Entry Point) and selects/clicks on a specific product item (e.g., "Vintage Rolex Submariner 1968"). | The user selects the item. | The system redirects the user to the product detail page, displaying the specific information of the selected item (LOT-2828-8142). | `1. [Detail] Implement Auction Product Detail Endpoint - Route GET /api/v1/auctions/:id - Return full Base64 compressed image, authenticity guarantee, and specs - Return allowed_increment_steps array` |
| **2** | The user is successfully redirected and is on the product detail page. | The page finishes loading. | The system must display: • Product Name: "Vintage Rolex Submariner 1968" • Auction Status: "Active" and "Bidding" • Time Countdown: Remaining auction time running (visible yellow box "Time remaining 00:04:32") • Current Price: "Current highest bid Rp12.500.000" along with highest bidder info. | `2. [SSE] Real-Time Room Event Listener & State Synchronization - Connect to /api/v1/sse/auctions/:id EventSource - Update current price, bidder rank, countdown timer, and live price chart` |
| **3** | The user is on the product detail page and scrolls to the product information area. | The page finishes loading. | The system displays the product detail information consisting of: • Menu Tabs: Details, Biders, Bid History, Documents. • About this lot: Description of product completeness. • Auction Rules: Displays Auction type, Starting price, Bid increment, Extension rule, and Lot ID. | `3. [Delivery] Product Detail Page Slicing & Offline Banner - Tabs layout (Details, Bidders, Bid History, Documents) - Connection interrupted banner with "Retry now" reconnect action` |
| **4** | The user is on the product detail page and looks below the description section. | The page finishes loading. | The system displays the Price History chart (bid price increase trend) complete with bidding timestamps and the total number of bids submitted. | `-` |
| **5** | The user is on the product detail page at the bottom section of the price history. | The page finishes loading | The system displays the latest Bid History list in real-time, including: • Bidding actions (e.g., "You bid Rp12.000.000") • Automatic time extension status ("Auction extended by 2 minutes") • Internet connection status ("Connection restored") | `-` |
| **6** | The user is on the product detail page and looks at the interaction panel on the right side. | The user wants to place a price bid. | The system displays the Place Bid form containing: • Bid amount input field (automatically pre-filled with the recommended minimum next bid, e.g., Rp12.600.000) • Main action button "Place bid - [Amount value]" | `-` |
| **7** | The user is on the product detail page at the bottom-right panel section. | The page finishes loading | The system provides a toggle activation option for "Autonomous Bidding" along with a "Maximum bid limit" input field and a "Set Auto Bid" confirmation button. | `-` |
| **8** | The user is actively viewing the product detail page for "Vintage Rolex Submariner 1968". | The system detects a network failure or WebSocket disconnection (Connection lost). | • The top navigation bar displays a red badge: "Connection interrupted". • A yellow warning banner appears below the header stating: "Live prices may be outdated. Last confirmed [Timestamp]. Reconnecting... Do not assume a submitted bid has failed or succeeded." alongside a "Retry now" button. • The Live indicator status component on the right panel changes to a red badge: "Connection interrupted". • The "My position" component displays a red text status: "Connection lost". • A pink information alert banner is injected in the bidding panel block stating: "Connection interrupted. Do not assume a submitted bid has failed or succeeded until its status is confirmed." | `-` |
| **9** | The network status warning banner and badges are visible due to a lost connection. | The user clicks the "Retry now" button on the yellow warning banner. | • The system attempts to re-initiate the WebSocket/API connection handshake. • If successful, the warning banners disappear, and data fields (Current highest bid, Time remaining, Bid history) refresh to their latest real-time states. | `-` |

---

### 14. [US-013] US-013 - Operations & Market Overview Dashboard

> **Story:** As a Bidder, Auction Manager, or Super Admin, I want to monitor the live bid stream and auction status in real time, so that Bidders can track market competition, AMs can oversee lot operations, and Super Admins can verify system integrity

**Sizing Appraisal:** Size **S** (Total Score: **90** | Tech Flow: 0 | AC Score: 75)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | The user (Bidder, AM, or Super Admin) is logged into the Auction Engine platform and opens the Overview Dashboard | The system establishes an active WebSocket/live network connection. | • The top header status pill component must dynamically render a green indicator showing "● Live • synced". • All numerical metrics, countdown timers, and tables must update asynchronously without requiring a manual browser refresh. | `-` |
| **2** | The Bidder is viewing the top grid panel of the dashboard workspace. | The background live sync data framework receives new transaction events. | The system must instantly display and balance check four core tracking metrics: • Available Balance: Shows ready-to-use funds (e.g., Rp48.250.000) with estimated changes underneath. • Locked Collateral: Highlights funds currently frozen for active bids (e.g., Rp9.500.000). • Active Bids: Displays total active count (e.g., 8) with a red warning subtext if user is outbid (e.g., 3 outbid / 1 highest). • Won this month: Shows successful historical outcomes. | `-` |
| **3** | The user has items saved to their personal auction watchlist | An item in the "Ending soon on your watchlist" slider receives a new bid or reaches a critical time state. | • The card overlay badge updates dynamically based on status (e.g., changing to a yellow blinking "🔥 Ending soon" or yellow "⏳ Extended" text badge). • The "Current bid" value flashes and updates instantly. • The "Time Remaining" countdown clock decrements perfectly every second. | `-` |
| **4** | The user (especially Bidder tracking market or AM overseeing lots) scrolls to the "My active bids" / "Lot Management" grid table. | Any status permutation transition occurs on a live item lot. | The system row must change state instantly, applying the correct color-coded visual indicator badges under the Status column: • Leading (Green): User currently holds the valid highest bid. • Outbid (Red): Another participant has overtaken the price matrix. • Extended (Orange): Anti-snipe time rule extension rules have kicked in. • Active (Blue): Open for standard bidding interaction. • Scheduled (Grey): Future lot item not yet open for live bids. | `-` |
| **5** | The Super Admin, AM, or Bidder looks at the "Recent activity" timeline list at the bottom container area. | Any transaction, system automated action, or network anomaly event is broadcasted. | The system must immediately append chronological system event lines with specific descriptive logging badges: • Bid Success (Green check): "Your bid Rp12.500.000 was accepted". • Outbid Notice (Red caution): "You have been outbid". • Auto-Bid Automation (Purple lightning): "Auto-bid executed Rp12.400.000". • Time Extension Rule (Yellow clock): "Auction extended by 2 minutes". | `-` |
| **6** | The system ledger detects that an active logged-in user has won an item but has not settled the invoice block. | The Overview dashboard page finishes loading. | • A high-priority blue systemic notice banner appears at the very top workspace grid section stating: "Payment due for 1 won auction. Settle by [Date/Timestamp]. Balance must clear...". • A functional "Pay now" action button is displayed on the far right edge of the alert element box to bypass and speed up the checkout settlement routing. | `-` |

---

### 15. [US-014] US-014 - View Lot Management List & Catalog Lifecycle (AM Exclusive)

> **Story:** User Story – View Lot Management List

As an AM, I want to view the list of lots in Lot Management, so that I can review and manage lot information according to my responsibilities.

**Sizing Appraisal:** Size **S** (Total Score: **65** | Tech Flow: 0 | AC Score: 50)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | The Auction Manager (AM) is logged into the Admin Workspace | The AM clicks on the "Lot Management" menu link option in the persistent sidebar. | • The system displays the main "Lot Management" workspace interface. • The top header status pill component must dynamically show a green indicator "● Live • synced". • The system displays a top contextual banner describing the catalog lifecycle: "Catalog of all lots. Draft → Scheduled → Active → Closed → Archived. Published lots lock critical fields." | `-` |
| **2** | The AM is viewing the upper control section of the Lot Management workspace. | The page finishes loading. | The system must render five lifecycle tab filters complete with active counters: • All (e.g., 312)  • Draft (e.g., 14)  • Scheduled (e.g., 18)  • Active (e.g., 24)  • Closed (e.g., 198)  • Archived (e.g., 58) | `-` |
| **3** | The AM wants to narrow down or rearrange the displayed lot records list grid. | The AM interacts with the filter drop-down components or the sort selector toggle. | • The UI provides drop-down components for Category (e.g., All), Type (e.g., All), Status, Price range, and Ending parameter metrics. • The system provides a "Sort: Ending soon" dropdown toggle list layout to control chronological row ordering layouts. | `-` |
| **4** | The AM is reviewing the primary catalog listing table view | The list records are fetched and populated. | The grid table must render the following specific structural data columns for each row: • Selection Checkbox: An interactive square component on the far left column margin. • Lot (Identity): Displays the item name asset title and the subtext unique Lot alphanumeric identifier code (e.g., "Rolex GMT-Master 1675", "LOT-2026-0161"). • Category: Renders the mapped asset grouping (e.g., Watches, Art, Cameras, Bags, Vehicles). • Type: Renders the auction mechanic format (e.g., Bounded Auction, Classic Auction). • Start Price: Displays standard formatted rupiah prices (e.g., Rp12.000.000). • Schedule: Renders planned duration windows or historical closed timelines (e.g., 31 Oct 09:00-14:00, Not scheduled, 20 Oct (ended)). | `-` |
| **5** | The AM is monitoring the active list rows under the Status column matrix. | A lot transitions across structural operational parameters. | The system row must change badge layout styling and text dynamically: • Active (Blue icon & text): Displays for items currently receiving live bids. • Scheduled (Grey folder icon): For finalized items awaiting launch windows. • Draft (Grey pencil icon): For incomplete items awaiting setup completion. • Ending soon (Orange clock badge): Critical time window phase warning status. • Extended (Orange clock badge): Triggered when anti-snipe buffer time kicks in. • Closed (Grey padlock icon): Triggered when the expiration timeline window concludes. | `-` |
| **6** | The AM looks at the far right column component margin of a specific item lot row. | The AM views the dynamic context link indicator or clicks the element tool trigger. | The system renders an operation text link mapped strictly to the item's live status state: • If status is Active, Ending soon, Extended, or Closed, display a "View" link to monitor live activities. • If status is Scheduled or Draft, display an "Edit" link to modify parameters. • If status is Closed (and meets retention rules), display an "Archive" link to shift the lot to storage records. | `-` |
| **7** | The catalog data records exceed the standard 8-row viewport page listing threshold interface. | The AM interacts with the pagination buttons at the bottom right corner | • The system renders a tracker stating current index metrics (e.g., "Showing 1-8 of 312 lots"). • Interactive navigation buttons labeled "Previous" and "Next" allow switching between table pages. | `-` |
| **8** | The AM is working actively anywhere inside the Lot Management table panel. | A global transaction milestone occurs on the platform (e.g., a bidder's offer is processed by the core ledger). | • The system slides out a real-time toast alert box at the top right workspace corner: "✓ Bid accepted". • The body layout of the toast details specific audit timestamps (e.g., "Your bid of Rp12.600.000 was accepted at 10:04:31.") with an interactive close (X) box icon. | `-` |

---

### 16. [US-015] US-015 - AM Live Auction Monitor & Authorized Interventions

> **Story:** As a AM I want to monitor the live bid stream and auction status in real time, so that Bidders can track market competition, AMs can oversee lot operations, and Super Admins can verify system integrity

**Sizing Appraisal:** Size **M** (Total Score: **140** | Tech Flow: 60 | AC Score: 50)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | The Auction Manager (AM) is logged into the Admin Workspace. | The AM clicks on the "Live Monitor" menu link option in the persistent sidebar. | • The system displays the "Live auction monitor" workspace layout. • The header sub-text must state: "Real-time view. Interventions require a reason and are written to the audit log. Bid outcomes cannot be edited." • The system status indicator must show a green badge "● Live - synced 10:04:31". • All widgets, streams, and graphs must process async data smoothly without requiring a manual window refresh. | `FE: Client initializes EventSource connection to /api/v1/sse/auctions/:id` |
| **2** | The AM is reviewing the list of ongoing lots on the left-hand panel titled "Active auctions (24)". | The AM clicks on a specific auction card item (e.g., "Hermès Birkin 30 Togo"). | • The selected auction item card highlights with a dark bold border outline. • The main workspace panel layout (Metrics, Price History, Bid Stream, and Interventions) instantly shifts context to render the real-time data of that selected lot. | `BE SSE Hub: Establish persistent HTTP/2 chunked response stream (text/event-stream)` |
| **3** | The AM is tracking the live operational metrics grid at the top center of the dashboard. | New real-time network packets and bids pass through the socket connection layer. | The system must instantly display and balance check three core live metrics counters: • Bids / 5 min: Shows incoming bid volume acceleration speed (e.g., 31 bids with indicator +24% vs 5 mins ago). • Unique bidders: Displays unique consumer participants (e.g., 19 active with subtext 4 using auto-bid). • Gateway p95: Monitors data latency platform response time (e.g., 820ms with threshold reference Target < 500ms). | `BE Engine: High-concurrency serialization in Redis Lua (< 5 ms execution)` |
| **4** | The AM looks at the top right section of the active lot monitor dashboard workspace. | The auction server clock ticks down or a bidder submits an accepted price increase. | • Current valuation: System prints large typography text "Rp214.000.000" next to a green "▲ 34" total bids milestone badge counter. • Time remaining: A yellow alarm banner box displays a real-time countdown timer tracking hours, minutes, and seconds (00:06:32) complete with anti-snipe buffer metrics subtext ("Extended +0:02 - ends at 10:08:41"). | `BE SSE Hub: Multiplex & dispatch price mutation frames to all active observers simultaneously` |
| **5** | The AM monitors the "Bid event stream" list feed in the center workspace. | Transactional webhooks or network connectivity state variations occur on the system. | The system must instantly append chronological updates complete with descriptive event color icon indicators: • Manual Bid (Blue hammer): "Bidder AA-2201 bid Rp12.500.000". • System Extension (Yellow clock): "Auction extended by 2 minutes - Anti-snipe rule". • Auto-Bid Action (Purple lightning): "Auto-bid placed Rp12.400.000 - Max proxy Rp15.000.000". • Network Status (Grey warning icon): "Connection restored - data resynced" | `Prometheus Exporter: Scrape telemetry & confirm latency SLA < 300 ms under 10k req/s load` |
| **6** | The AM reviews the "System health" tracking sidebar panel on the right margin. | The infrastructure manager framework tracks concurrent users and drop statistics. | The table must continuously print diagnostic metadata parameters containing: • WebSocket channels: Total active connected clients (e.g., 1,254 connected). • Bid gateway p95: Instant latency tracker (e.g., 820ms). • Event log: Throughput frequency speed tracking metrics (e.g., 120ms). • Packet loss (%): Operational drop safety checker indicator (e.g., 0). | `-` |
| **7** | The AM is reviewing an active auction experiencing anomalous patterns or urgent issues. | The AM triggers an entry point button inside the "Authorized interventions" section at the bottom center. | The system exposes four operational mitigation action buttons: • "⏸ Pause 5 min": Halts incoming bids and freezes timers temporarily. • "🏳 Flag bidder": Opens an interface to penalize a bad actor. • "💬 Notify bidders": Broadcasts a system banner message alert layout. • "🛑 Cancel auction...": A red primary button that opens a confirmation window requiring an explicit reason text log input to permanently terminate the active lot event. | `-` |

---

### 17. [US-016] US-016 - Participants Directory & KYC Compliance Ledger (AM Exclusive)

> **Story:** As a AM I want to monitor the live bid stream and auction status in real time, so that Bidders can track market competition, AMs can oversee lot operations, and Super Admins can verify system integrity

**Sizing Appraisal:** Size **M** (Total Score: **140** | Tech Flow: 60 | AC Score: 50)

| No | Given | When | Then | Technical Flow |
|:---|:---|:---|:---|:---|
| **1** | The Auction Manager (AM) is logged into the Admin Workspace and opens the "Participants" view panel page. | The page finishes loading. | The system must instantly render four core analytical stats tracking counters: • Verified bidders: Displays total count (e.g., 3.412) with weekly speed growth indicators (▲ +85 this week). • Pending verification: Tracks items awaiting KYC process checks (e.g., 27 with subtext Avg. 4h to review). • With active collateral: Tracks locked funds currently frozen for active bids (e.g., 1.108 participants with total value metadata Rp4.2 B total). • Restricted: Highlights account blocks (e.g., 6 with an urgent red notification badge ⚠ 2 new today). | `FE: Client initializes EventSource connection to /api/v1/sse/auctions/:id` |
| **2** | The AM wants to target a specific subset segment array of the customer directory ledger. | The AM selects items inside the filter drop-down menus (Category, Type, Status, Price range, Ending). | • The system generates removable active blue tag filter blocks under the header margin layout (e.g., Category: Watches ✕, Type: Bounded Auction ✕). • The central table row rows instantly clear and refresh asynchronously to isolate data records matching the active conditions grid. | `BE SSE Hub: Establish persistent HTTP/2 chunked response stream (text/event-stream)` |
| **3** | The AM is on the persistent main directory table page dashboard space. | The AM clicks the "Export" functional action button link block located at the top-right container workspace border area. | • The system aggregates the active data grid entries array mapping (including Bidder ID, Verification status, Collateral value, Active bids, Won metrics, and Location data). • The system bundles the backend query dataset and dispatches a file takeout download package matching structural standard formatting criteria (.CSV or .XLSX). | `BE Engine: High-concurrency serialization in Redis Lua (< 5 ms execution)` |
| **4** | The active database registers are successfully fetched and pulled to the dashboard view viewport. | The page layout rendering process initiates. | The directory table list grid layout template container object container components must print exactly six systematic operational meta data columns: • Bidder (Identity): Displays Customer Full Name, unique alphanumeric hashtag hash tracker parameter index tag, and geographic residential base city location metrics (e.g., "Ayya Adzani", "#B-1042", "Jakarta"). • Verification: Prints the structural compliance state identifier validation stamp badge row configuration. • Collateral: Formats current frozen escrow financial assets balance (e.g., Rp25.000.000 / -). • Active Bids: Displays active concurrent transaction ticket counters (e.g., 14). • Won /90th: Total items awarded successfully over a rolling 90-day time scope ledger interval window. • Last Active: Displays time lapse updates relative to system server timestamps (e.g., Now, 2 min ago, 1 h ago, 12 Oct). | `BE SSE Hub: Multiplex & dispatch price mutation frames to all active observers simultaneously` |
| **5** | The AM is inspecting the active database list grid table layout view components | The background compliance validation layer switches account flags or user updates documentation files. | The system column records container must instantly re-style visual badge assets to conform to specific compliance micro-states: • Verified (Green check): Confirms verified authentication checks status clear credentials. • Under review (Red caution): Warns about security risk or pseudonymized accounts awaiting auditing (e.g., "Bidder #A-2291"). • Pending (Orange clock): New onboarding KYC records queue state pipeline target link. • Restricted (Grey padlock): Accounts penalized under risk mitigation compliance criteria guidelines rules. | `Prometheus Exporter: Scrape telemetry & confirm latency SLA < 300 ms under 10k req/s load` |
| **6** | The AM looks at the operational action command button alignment block on the extreme right border margin area of an item row. | The user table row renders on screen. | The component maps and renders specific action control buttons mapped strictly to the target participant's security verification state: • If verification is Verified or Restricted, show a subtle text link option button labeled "View" to check account sheets. • If verification is Under review, display an active button component labeled "Review" to force inspection parameters. • If verification is Pending, display a primary dark-blue actionable button block component labeled "Verify" to invoke standard credentials authorization. | `-` |

---

