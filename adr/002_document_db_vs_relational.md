# Document Database (Cosmos DB) vs Relational Database

- **Status:** Accepted  
- **Date:** 2026-02-16  
- **Decision Drivers:** Iteration speed, tenant isolation, operational simplicity, scalability

---

## Context

The system requires:

- Rapid schema evolution
- Tenant-scoped access patterns
- Managed operational model
- Predictable performance at scale

The primary decision: use a document database or a relational database.

---

## Decision

Use Azure Cosmos DB with TenantId as partition key.

---

## Rationale

Cosmos DB provides:

- Flexible schema evolution
- Partitioning aligned with tenant-based access
- Managed availability and scaling
- Reduced operational overhead

---

## Alternatives Considered

### Relational Database (Postgres / SQL Server)

**Pros:**
- Strong relational integrity
- Mature tooling
- Complex joins supported

**Cons:**
- Schema rigidity
- Horizontal scaling more complex
- Potential slower iteration

**Reason not chosen:** Tenant-partitioned document access better matches workload patterns.

---

## Security & Compliance Considerations

- TenantId partitioning enforces logical data isolation
- Access controls enforced at API boundary
- Reduced surface area compared to self-managed DB

Risk: Cross-partition queries may expose data if not properly scoped.

Mitigation:
- Enforce tenant context in middleware
- Restrict cross-partition queries

---

## Trade-offs / Consequences

### Benefits
- Faster feature iteration
- Scalable tenant-aware model
- Reduced ops burden

### Costs / Risks
- Requires disciplined partition design
- Less natural support for relational joins

### Mitigations
- Strict data access patterns
- Performance monitoring on partition usage

---

## Validation / Revisit Criteria

Reassess if:

- Complex relational reporting dominates workload
- Cross-entity transactional guarantees become primary requirement

---

## Outcome

The document database aligns with tenant-scoped SaaS patterns and supports rapid product evolution while maintaining operational simplicity.

