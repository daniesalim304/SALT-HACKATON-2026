# Documentation Lifecycle Methodology

Guidelines for managing the authoring, peer review, approval, and eventual deprecation of artifacts.

---

## 1. Lifecycle Stages

```mermaid
stateDiagram-v2
    [*] --> Draft: Authoring
    Draft --> In_Review: Technical Defense Ready
    In_Review --> Approved: Stakeholder Sign-Off
    In_Review --> Draft: Revisions Needed
    Approved --> Superseded: Replaced by New Version
    Approved --> Archived: Project Closed
    Superseded --> Archived: Final Storage
```

* **Transition Rule:** A document transitions from `Draft` to `In Review` only when all required template sections are populated without pending placeholders.
* **Traceability Update:** When a document becomes `Superseded`, an explicit warning banner with a link to the successor must be placed immediately under the title header.
