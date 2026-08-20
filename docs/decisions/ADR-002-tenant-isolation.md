# ADR-002: Enforce Tenant Isolation in Application and Database Layers

- **Status:** Accepted for the reference foundation
- **Context:** Application-only tenant filters are vulnerable to missing predicates and privileged code paths.
- **Decision:** Propagate a validated tenant context through the request and transaction lifecycle, then enforce matching PostgreSQL Row-Level Security policies under least-privilege runtime roles.
- **Consequences:** Cross-tenant exposure requires failure at more than one boundary. Tests and connection management must verify that tenant context cannot leak between requests.
