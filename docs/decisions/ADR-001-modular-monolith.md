# ADR-001: Start with a Modular Monolith

- **Status:** Accepted for the reference foundation
- **Context:** The platform needs strong domain separation but does not yet have operational evidence that justifies distributed deployment and data ownership.
- **Decision:** Keep bounded contexts in one deployable application with enforced module interfaces.
- **Consequences:** Transactions, testing and local operations remain simpler. Module discipline is required to prevent accidental coupling. Services may be extracted only when scaling, ownership or resilience evidence supports the change.
