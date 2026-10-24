# Change Management Standards

This document establishes the protocol for modifying existing baselines, tracking evolutionary changes, and retiring documents without knowledge loss.

---

## 1. Principles of Change Control
1. **Never Silently Overwrite:** Existing requirements, architectural decisions, and approved designs represent hard-won alignment. Any modification must provide a rationale and update the document version.
2. **Impact Assessment:** Prior to modifying an established document, assess downstream impacts on dependent components (e.g. modifying an API spec impacts data models and frontend contracts).
3. **Traceability of Superseded Material:** When an architectural direction is reversed or replaced, do not delete the old decision record. Create an ADR marked `Superseded` with an explicit reference to the new ADR.

---

## 2. Versioning Scheme
Follow Semantic Versioning principles for formal documents:
- **Major (v1.0.0, v2.0.0):** Significant scope re-baselining, fundamental architectural redesign, or major milestone approval.
- **Minor (v0.2.0, v1.1.0):** Addition of new modules, functional requirements, or updated integration flows.
- **Patch (v0.1.1, v1.0.1):** Typo fixes, minor clarifications, or diagram formatting updates that do not alter technical behavior.

---

## 3. Archival Workflow
When a file, feature, or project becomes inactive or obsolete:
1. Mark the header status as `Superseded` or `Archived`.
2. Add a banner at the top linking to the active successor.
3. Move files to the appropriate `99_Archive/` directory only after all active references have been redirected or removed.
