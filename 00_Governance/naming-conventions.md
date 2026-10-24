# Naming Conventions

Consistent naming allows human engineers and automated agents to immediately infer context, type, and ordering without opening files.

---

## 1. Directory Naming
- **Root Folders:** Numbered two-digit prefix followed by CamelCase or SnakeCase:
  `00_Governance`, `01_Inbox`, `02_Projects`, `03_Knowledge_Base`, `04_Templates`, `05_Methodologies`, `06_Reference`, `99_Archive`.
- **Project Folders:** Stable, lowercase kebab-case or concise Title_Case:
  `02_Projects/real-time-auction-engine/`
- **Internal Project Subdirectories:** Standardized numbered sections:
  `00_Project_Overview`, `01_Source_Documents`, `02_Analysis`, `03_Requirements`, `04_Design`, `05_Integration`, `06_Data_Design`, `07_Testing`, `08_Operations`, `09_Decisions`, `99_Archive`.

---

## 2. File Naming
- **General Documentation & Knowledge Base:** Lowercase `kebab-case.md` (e.g. `rate-limiting-strategies.md`, `project-intake-workflow.md`).
- **Formal Project Deliverables:** Clear descriptive identifiers with optional document code (e.g. `srs-system-requirements-specification.md`, `hld-high-level-design.md`, `api-contracts.md`).
- **Decision Records:** `adr-XXX-title.md` (e.g. `adr-001-redis-distributed-lock.md`).

---

## 3. Identifiers in Requirements & Architecture
To maintain end-to-end traceability across systems, use stable sequential prefixes:

| Category | Identifier Pattern | Example |
|---|---|---|
| Functional Requirement | `FR-XXX` | `FR-001: User Collateral Lock` |
| Non-Functional Requirement | `NFR-XXX` | `NFR-001: Sub-300ms Bid Latency` |
| Business Rule | `BR-XXX` | `BR-001: Minimum Bid Increment` |
| Integration Requirement | `IR-XXX` | `IR-001: WebSocket Bid Distribution` |
| Data Requirement | `DR-XXX` | `DR-001: Immutable Bid Audit Log` |
| Architecture Decision | `ADR-XXX` | `ADR-001: Go Sync vs Redis Mutex` |
| Clarification Question / Gap | `GAP-XXX` / `Q-XXX` | `GAP-001: Anti-Sniping Window Duration` |
| Feature Identifier | `FEAT-XXX` | `FEAT-001: Automated Proxy Bidding` |
