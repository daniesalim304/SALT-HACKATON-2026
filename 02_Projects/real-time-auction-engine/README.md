# Project: High-Frequency Real-Time Auction & Bidding Engine

| Metadata | Nilai |
|---|---|
| **Domain** | Fintech / E-Commerce (High-Concurrency Auction Engine) |
| **Status Proyek** | `In Design` (Tahap Perancangan MVP 1) |
| **Konteks** | Hackathon 2026 Official Guidelines |
| **Stack Wajib** | Next.js, Golang, PostgreSQL, Redis, WebSockets |
| **Dokumen Sumber** | `01_Source_Documents/hackathon-2026-guidelines.pdf` |

---

## 1. Deskripsi Proyek
Platform lelang aset bernilai tinggi (*high-value assets*) yang membutuhkan lingkungan penawaran digital yang transparan, andal, dan berlatensi sangat rendah (< 300 ms). Platform ini mendukung dua model lelang (*Capped Value* dan *Uncapped*), mekanisme penguncian agunan likuiditas (*collateral hold*), penawaran otomatis (*proxy bidding*), dan perpanjangan waktu dinamis (*anti-sniping*).

---

## 2. Struktur Direktori Proyek

```text
02_Projects/real-time-auction-engine/
├── README.md                      # Index dan status proyek (file ini)
├── 00_Project_Overview/           # Piagam proyek, batasan ruang lingkup, matriks persona
├── 01_Source_Documents/           # Salinan dokumen sumber & brief resmi
├── 02_Analysis/                   # Analisis kebutuhan, gap log, dan inventaris fitur
├── 03_Requirements/               # Spesifikasi kebutuhan (SRS, FR, NFR, BR)
├── 04_Design/                     # Perancangan sistem (User Flow per fitur, Sequence Diagram, HLD, LLD)
├── 05_Integration/                # Kontrak API (RESTful & WebSocket Protocol)
├── 06_Data_Design/                # Skema Database (PostgreSQL) & Struktur Data Cache (Redis)
├── 07_Testing/                    # Skenario pengujian TDD, edge cases, test cases
├── 08_Operations/                 # Panduan deployment & observabilitas performa
├── 09_Decisions/                  # Architecture Decision Records (ADR)
└── 99_Archive/                    # Dokumen usang / riwayat revisi
```

---

## 3. Indeks Dokumen Desain & Analisis Aktif

* [03_Requirements/user-stories-and-ac.md](03_Requirements/user-stories-and-ac.md) — Koleksi User Stories & Acceptance Criteria Lengkap (v2.0 Baseline).
* [04_Design/ifa-initial-functional-analysis.md](04_Design/ifa-initial-functional-analysis.md) — Initial Functional Analysis (IFA) Dekomposisi Modul Fungsional & RBAC Matrix.
* [04_Design/high-level-design.md](04_Design/high-level-design.md) — High-Level Design (HLD) Topologi Sistem 4 Lapisan & ADR.
* [04_Design/lld-low-level-design.md](04_Design/lld-low-level-design.md) — Low-Level Design (LLD) Structs Go, Usecase Logic, WebSocket Hub & Endpoint List.
* [04_Design/user-flows-mvp1.md](04_Design/user-flows-mvp1.md) — Alur Pengguna Mendalam per Fitur (FEAT-001 s/d FEAT-010).
* [04_Design/sequence-diagrams-mvp1.md](04_Design/sequence-diagrams-mvp1.md) — Diagram Urutan Interaksi Sistem Fase 1 (SLA < 300 ms).
* [06_Data_Design/database-schema.md](06_Data_Design/database-schema.md) — Perancangan Skema PostgreSQL DDL & Redis Lua Scripting.
* [05_Integration/api-contracts.md](05_Integration/api-contracts.md) — Koleksi Kontrak API Lengkap (14 REST Endpoints dengan Request/Response JSON & WebSocket).
* [07_Testing/test-plan-and-cases.md](07_Testing/test-plan-and-cases.md) — Rencana Pengujian TDD, Matriks Kasus Uji, dan Uji Konkurensi Masif.
