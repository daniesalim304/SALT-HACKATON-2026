# 04 Templates

This directory houses authoritative, standardized Markdown templates for deliverables across the project lifecycle.

---

## 1. Directory Structure

```text
04_Templates/
├── README.md                  # Template directory index and usage guidelines (this file)
├── Project_Intake/            # Templates for Project Charters, Readmes, and Overview decks
├── Requirements/              # Templates for BRD, SRS, Feature Catalogues, and Traceability Matrices
├── Architecture/              # Templates for IFA, HLD, LLD, and Component Designs
├── Integration/               # Templates for REST API Contracts, WebSocket Protocols, Webhooks
├── Data_Design/               # Templates for Data Dictionaries, ERD specs, and Schema Definitions
├── Testing/                   # Templates for Test Plans, Test Cases, and Defect Logs
└── Decision_Records/          # Templates for Architecture Decision Records (ADRs)
```

---

## 2. Usage Instructions

* When creating a new project deliverable, copy the appropriate template into the project's corresponding directory (e.g. `04_Templates/Architecture/template-hld.md` $\rightarrow$ `02_Projects/<project>/04_Design/hld.md`).
* Replace all bracketed placeholders `[Placeholder]` with project-specific content.
* Never edit templates directly for project-specific needs; maintain templates as reusable, generic baselines.
