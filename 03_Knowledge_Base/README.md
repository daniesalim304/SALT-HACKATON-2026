# 03 Knowledge Base

The **Knowledge Base** stores curated, cross-cutting engineering knowledge, architectural patterns, and reusable technical insights that are independent of any single project.

---

## 1. Directory Structure

```text
03_Knowledge_Base/
├── README.md                  # Index of reusable engineering domains (this file)
├── Architecture_Patterns/     # Concurrency models, event-driven designs, caching strategies
├── System_Analysis/           # Requirement elicitation methods, domain modeling practices
├── Integration_Patterns/      # WebSockets, gRPC, API gateways, idempotency & retry mechanics
├── Data_Engineering/          # High-throughput storage, sharding, transactional integrity (ACID)
├── Security/                  # Token authentication, rate limiting, anti-abuse, authorization
├── Infrastructure/            # Containerization, Kubernetes patterns, CI/CD pipelines
└── Troubleshooting/           # Post-mortems, recurring concurrency bugs, diagnostic runbooks
```

---

## 2. Promotion Policy (Project $\rightarrow$ Knowledge Base)

Knowledge is promoted here only when:
1. It has been validated in at least one real-world project context.
2. It has been distilled to remove company-confidential or project-specific data.
3. It represents a reusable pattern, standard, or proven architectural solution.
