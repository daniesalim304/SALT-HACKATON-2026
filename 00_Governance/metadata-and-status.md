# Metadata and Status Standards

Standardized document metadata ensures that any collaborator or AI agent can immediately ascertain the lifecycle state, authoritativeness, and currency of an artifact.

---

## 1. Document Metadata Header Schema

Place the following table at the very top of formal project deliverables:

```markdown
| Metadata | Value |
|---|---|
| **Document Title** | [Title of Document] |
| **Project** | [Project Name] |
| **Document Type** | [SRS / HLD / LLD / IFA / API Spec / ADR / Analysis] |
| **Version** | v0.1.0 (Draft) |
| **Status** | [Draft / In Review / Approved / Superseded / Archived] |
| **Created Date** | YYYY-MM-DD |
| **Last Updated** | YYYY-MM-DD |
| **Author / Lead** | [System Analyst / Solution Architect / Team] |
| **Source Reference** | [Upstream Document or Path] |
```

---

## 2. Document Lifecycle Statuses

* **`Draft`**: Initial working document. Incomplete, actively undergoing edits, not yet validated.
* **`In Review`**: Complete working version submitted to stakeholders or peer architects for technical defense and review.
* **`Approved`**: Formally validated by stakeholders / technical leads. Serves as an authoritative baseline for implementation.
* **`Superseded`**: Replaced by a newer version or successor document. Retained for historical context with a link to the replacement.
* **`Archived`**: Abandoned, decommissioned, or completed project artifact stored permanently in `99_Archive/`.

*Rule:* Never mark an artifact `Approved` purely because an AI or engineer generated it. Approval requires explicit human or stakeholder validation.
