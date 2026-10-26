# 02 Projects

This directory contains the authoritative, isolated knowledge bases for all active and completed initiatives within the Second Brain.

---

## 1. Directory Structure Conventions

Each project resides within its own dedicated subdirectory under `02_Projects/<project-name>/`.

### Standard Project Subdirectory Scaffold:

```text
02_Projects/<project-name>/
├── README.md                  # Project index, objectives, status, team, and artifact links
├── 00_Project_Overview/       # Project charter, context, scope boundaries, stakeholder matrices
├── 01_Source_Documents/       # Original PDFs, briefs, customer transcripts, and immutable inputs
├── 02_Analysis/               # Phase 1-5 analysis reports, clarification logs, feature inventories
├── 03_Requirements/           # SRS, BRD, functional/non-functional requirements, business rules
├── 04_Design/                 # IFA, HLD, LLD, architecture diagrams, component specs
├── 05_Integration/            # REST/SSE API contracts, third-party schemas, webhooks
├── 06_Data_Design/            # ERDs, database schemas, data dictionaries, migration plans
├── 07_Testing/                # Test plans, test cases, automated TDD scenarios, defect reports
├── 08_Operations/             # Deployment guides, runbooks, monitoring dashboards, CI/CD specs
├── 09_Decisions/              # Architecture Decision Records (ADRs)
└── 99_Archive/                # Superseded or retired project-specific artifacts
```

*Note:* Subfolders should be provisioned as the project progresses. Do not generate empty subfolders indiscriminately if not immediately needed.

---

## 2. Project List

| Project Name | Folder Name | Status | Primary Focus |
|---|---|---|---|
| *(Template / Inactive)* | *(Refer to `04_Templates/Project_Intake/`)* | - | - |

*(New projects are initialized here following `05_Methodologies/project-intake-workflow.md`)*
