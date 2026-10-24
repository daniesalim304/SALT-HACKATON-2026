# Project Intake Workflow

This workflow guides the structured intake of a new project into the Second Brain workspace.

---

## Process Steps

```mermaid
flowchart TD
    A["Raw Material Received (PDF, Brief, RFP)"] --> B["Step 1: Deposit into 01_Inbox/"]
    B --> C{"Is it a new project or existing?"}
    C -->|Existing Project| D["Move to 02_Projects/<project>/01_Source_Documents/"]
    C -->|New Project| E["Step 2: Initialize Project Scaffold in 02_Projects/<project-name>/"]
    E --> F["Step 3: Transfer Source Artifacts to 01_Source_Documents/"]
    F --> G["Step 4: Author Project README.md (Charter, Scope, Status)"]
    G --> H["Step 5: Trigger PDF / Requirement Analysis Workflow"]
    D --> H
```

### Step 1: Ingestion
Receive raw files into `01_Inbox/PDFs/` or relevant inbox subdirectories.

### Step 2: Provision Project Directory
Create directory: `02_Projects/<project-name>/` with standardized core folders (`00_Project_Overview/`, `01_Source_Documents/`, `02_Analysis/`, `03_Requirements/`, `04_Design/`, `09_Decisions/`).

### Step 3: Populate Project README
Initialize `02_Projects/<project-name>/README.md` defining project purpose, business context, active status (`Draft`), and key artifact links.
