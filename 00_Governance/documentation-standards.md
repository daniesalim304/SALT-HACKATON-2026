# Documentation Standards

This standard specifies the structural, visual, and qualitative rules required for all technical deliverables within the Second Brain.

---

## 1. Document Structure & Consistency
Every formal document must follow a logical flow:
1. **Metadata Header:** Standardized table containing Title, Project, Version, Status, Date, and Source References.
2. **Executive Summary / Purpose:** Concise orientation paragraph answering *what* and *why*.
3. **Core Body:** Structured sections with clear headings and numbered lists.
4. **Traceability / References:** Links to upstream source documents and downstream artifacts.

---

## 2. Diagram Standards
Diagrams must be generated using text-based **Mermaid** blocks embedded directly in Markdown files:
- **System Boundaries & Context:** C4 Context or Flowcharts (`flowchart TD` / `flowchart LR`).
- **State Transitions:** State diagrams (`stateDiagram-v2`) for lifecycles (e.g. auction lot states, order states).
- **Sequence of Interactions:** Sequence diagrams (`sequenceDiagram`) for multi-system or high-concurrency flows.
- **Data Structures & Models:** Entity-Relationship diagrams (`erDiagram`).

*Rule:* All diagram components must align exactly with the accompanying prose and data dictionaries. Do not introduce phantom services or unverified interfaces.

---

## 3. Fact vs. Assumption Separation
Never blur confirmed facts with recommendations. Use explicit formatting callouts:
- **[Confirmed]:** Sourced directly from verified specifications or stakeholder sign-off.
- **[Assumption]:** Working hypothesis adopted to prevent deadlock; requires validation.
- **[Proposed]:** Architect recommendation under active review.
- **[Open Question]:** Unresolved unknown blocking or influencing technical decisions.
