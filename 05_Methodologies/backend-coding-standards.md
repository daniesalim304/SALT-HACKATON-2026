# Backend Coding Standards & Architectural Guidelines

This document defines the mandatory architectural patterns, coding standards, and quality requirements for this Go backend codebase. All agents and developers must strictly adhere to these guidelines.

---

## 1. Clean Architecture & Layer Boundaries

Dependencies flow strictly inward:

```
Delivery → Application → Domain ← Infrastructure
```

| Layer | Path | Responsibilities & Allowed Dependencies |
|---|---|---|
| **Domain** | `domain/` | Core business logic, entities, value objects, domain events, domain repository interfaces. **Zero external dependencies**, no infrastructure or delivery imports. |
| **Application** | `application/` | Use cases, workflow orchestration, DTOs. Depends **only** on the Domain layer. Must **never** import `infrastructure/` directly. |
| **Infrastructure** | `infrastructure/` | Implements domain repository and service interfaces (Postgres, Redis, external APIs). Depends on Domain and Application interfaces. |
| **Delivery** | `delivery/` | Transport layer (HTTP handlers, routing, middleware). Translates HTTP requests to use case inputs and maps responses/errors. Depends on Application. |
| **Pkg** | `pkg/` | Shared utility packages and cross-cutting types (e.g. `pkg/errs` for error definitions). |

---

## 2. Domain Layer Guidelines (Pragmatic DDD)

- **Rich Domain Model**: Business logic belongs in domain entities and value objects, never leaked into use cases or handlers.
- **Entity Structs & Public Fields**: Invariants are protected through behavior and validated constructors, not unexported fields.
- **Validated Constructors**:
  - Untrusted input (from use-case commands) **must** go through a `New<Entity>(...) (*Entity, error)` constructor that validates business invariants and returns an error if invalid.
  - Reconstitution from trusted persistence (repositories) uses direct struct literals without calling the constructor.
- **Ubiquitous Language**: Method names must reflect domain operations (e.g. `user.SyncProfile(...)`, `user.Deactivate()`, `user.DemoteOrPromote(...)`), not generic setters (`SetEmail`, `UpdateField`).
- **One Entity / Value Object Per File**: Single Responsibility Principle; keep types and their methods in the same file.
- **Financial Datatypes**: All monetary and balance calculations **MUST** use `float64`.

---

## 3. Application Layer Guidelines

- **Thin Orchestrators**: Use cases only coordinate workflows:
  1. Fetch entities from repositories
  2. Invoke domain methods / invariants
  3. Persist changes via repository interfaces
  4. Map results to response DTOs
- **Constructor Pattern**: Use cases must use unexported structs with exported constructor functions returning interfaces:
  ```go
  type AuthLoginUseCase interface {
      Execute(ctx context.Context, input AuthLoginInput) (*AuthLoginOutput, error)
  }

  type authLoginUseCase struct {
      oidcService domain.OIDCService
  }

  func NewAuthLoginUseCase(oidcService domain.OIDCService) AuthLoginUseCase {
      return &authLoginUseCase{oidcService: oidcService}
  }
  ```
- **Error Classification**: The use case is the **only** place that classifies errors as business vs. technical using `pkg/errs`:
  - Business errors (`errs.NewBusinessError(errs.AUTH_INVALID_STATE, err)`)
  - Technical errors (`errs.NewTechnicalError(errs.INTERNAL_SERVER_ERROR, err)`)

---

## 4. Infrastructure & SQL Standards

- **Raw SQL is the Default**: At least 70% of repository implementations should use raw SQL with parameterized queries (`$1, $2, ...` in PostgreSQL). Never concatenate or interpolate parameters.
- **Context Propagation**: Always use `QueryContext`, `ExecContext`, and pass `ctx` through all database operations.
- **No Serialization Tags on Infra Models**: Do not put `json` tags on database models or expose them directly as API responses. Map them to domain entities.
- **Naming Conventions**:
  - Tables: plural `snake_case` (e.g., `users`, `products`).
  - Columns: `snake_case` (e.g., `niro_sub`, `created_at`).
  - Indexes: `snake_case` with `_INDEX` suffix (e.g., `"users_niro_sub_INDEX"`).
  - Primary Keys: UUID or big integer.
  - Timestamps: `created_at` and `updated_at` are mandatory.

---

## 5. Delivery Layer & Error Handling

- **JSON API Response Envelope**: All API endpoints must adhere to the standardized response envelope:
  ```json
  {
    "status": "success",
    "data": { ... }
  }
  ```
  And on error:
  ```json
  {
    "status": "error",
    "error": {
      "code": "AUTH_INVALID_STATE",
      "message": "invalid or expired transit state"
    }
  }
  ```
- **HTTP Status Mapping**:
  - Business errors → `4xx` (e.g., `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`).
  - Technical errors → `500 Internal Server Error`.
  - Never expose database driver errors or raw stack traces in responses. Log the technical details server-side.
- **Error Codes**: Descriptive `UPPER_SNAKE_CASE` constants registered in `pkg/errs`.

---

## 6. Code Style & File Limits

- **File Size**: Keep source files under **300 lines**. Splitting is mandatory at **500 lines**.
- **KISS & YAGNI**: Avoid speculative abstractions, generic wrappers with single usages, or dead code.
- **Go Idioms**:
  - Use `any` instead of `interface{}`.
  - Multi-line `const (...)` and `var (...)` groupings.
  - Return error as the last return parameter.
  - Run `gofmt` and ensure code passes linters cleanly.

---

## 7. Testing Requirements

- **TDD Expected for Domain and Application**:
  - Unit test coverage is strictly required for `domain/` and `application/`.
  - Integration tests live in `test/infrastructure/`.
- **Testing Tools**:
  - Use [Testify](https://github.com/stretchr/testify) (`assert`, `require`).
  - Table-driven test structure for covering multiple scenarios.
  - Separate test packages with `_test` suffix (e.g. `package entity_test`, `package usecase_test`) placed beside the source files.
  - Deterministic and isolated: never share mutable pointers across test cases.
