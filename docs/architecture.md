# Architecture Notes

## Bounded contexts

The reference platform separates the following responsibilities:

| Context           | Responsibility                                   | Primary control               |
| ----------------- | ------------------------------------------------ | ----------------------------- |
| Identity & Access | Users, organizations, roles and sessions         | Tenant-aware authorization    |
| Asset Registry    | Canonical asset records and lifecycle state      | Validated state transitions   |
| Compliance        | Eligibility and approval workflow hooks          | Explicit review gates         |
| Tokenization      | Issuance intent and chain-operation coordination | Idempotency and policy checks |
| Audit             | Immutable business-event evidence                | Append-only permissions       |

These are logical boundaries inside a modular monolith, not claims of independently deployed services.

## Transaction boundary

```mermaid
sequenceDiagram
  participant U as Institutional user
  participant API as Application API
  participant T as Tenant policy
  participant D as Domain service
  participant DB as PostgreSQL RLS
  participant A as Audit log

  U->>API: Submit state change
  API->>T: Resolve identity and tenant
  T-->>API: Authorized context
  API->>D: Execute domain command
  D->>DB: Transaction under tenant context
  DB-->>D: Enforced result
  D->>A: Append business event
  D-->>U: Traceable outcome
```

## External integrations

Custody, identity verification, payment rails and blockchain providers must sit behind explicit ports. Provider payloads are validated and translated into domain types so third-party schemas do not become the core model.
