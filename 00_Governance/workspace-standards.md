# Workspace Standards

## 1. Operating Rules
- **Markdown-First:** All editable knowledge must be maintained in standard GitHub Flavored Markdown (`.md`).
- **Relative Linking:** Internal references across files must use standard relative links (e.g. `../03_Requirements/srs.md`).
- **Single Source of Truth:** Do not duplicate authoritative requirement or design text across multiple files. Maintain the master in its dedicated directory and link to it.

## 2. Directory Separation
- **Governance vs. Project:** Universal rules stay in `00_Governance/`. Project-specific rules must stay within `02_Projects/<project>/`.
- **Project Isolation:** Project spaces in `02_Projects/` are strictly isolated. No project should directly depend on or mutate files in another project's folder.
- **Promotion to Knowledge Base:** When a solution design yields a generalized design pattern or reusable architectural insight, distill it and place it into `03_Knowledge_Base/`, referencing the originating context.
