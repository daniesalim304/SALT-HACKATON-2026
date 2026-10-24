# 01 Inbox

The **Inbox** is the temporary intake zone for raw, incoming, and unclassified artifacts before they are analyzed and filed into their permanent project directories.

---

## 1. Directory Structure

```text
01_Inbox/
├── README.md              # Intake guidelines and triage instructions (this file)
├── PDFs/                  # Product briefs, specifications, whitepapers, RFPs
├── Documents/             # Word documents, text files, raw meeting minutes, interview notes
├── Spreadsheets/          # Financial models, legacy data dumps, user lists, metric exports
├── Diagrams/              # Raw architecture exports, sketches, legacy visio/draw.io files
└── Other/                 # Uncategorized or miscellaneous binary assets
```

---

## 2. Intake Rules & Triage Flow

1. **Transient by Design:** The Inbox is not a long-term storage folder. Materials placed here should be triaged into `02_Projects/<project>/` or `06_Reference/`.
2. **Preserve Integrity:** Never modify the content of a raw source file in the Inbox.
3. **Triage Checklist:**
   - Determine if the asset belongs to an existing project in `02_Projects/` or initiates a new project.
   - If initiating a new project, create the project directory using the standard project scaffold.
   - Move or copy the source document to `02_Projects/<project>/01_Source_Documents/`.
   - Once safely copied and cataloged in the project manifest, the intake copy may be archived or removed.
