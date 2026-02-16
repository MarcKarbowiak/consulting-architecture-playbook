# Technical Design Doc: <Project / Feature Name>

- **Author:** <Name>
- **Date:** YYYY-MM-DD
- **Status:** Draft | In Review | Approved
- **Target Release:** <date/version>
- **Stakeholders:** Product, Engineering, Security, Ops
- **Related ADRs:** <Links if applicable>

---

## 1. Executive Summary

Concise description of what is being built, why it matters, and the expected impact.

---

## 2. Problem Statement

What problem are we solving?

- Business context
- User impact
- Current limitations
- Risks of not solving it

---

## 3. Goals and Non-Goals

### Goals
- …

### Non-Goals
- Explicitly state what this design will NOT address.

---

## 4. Requirements

### Functional Requirements
- …

### Non-Functional Requirements
- Availability (SLO targets if known)
- Performance (latency/throughput)
- Scalability expectations
- Security requirements
- Compliance / data residency
- Cost constraints
- Observability expectations

---

## 5. Current State (If Applicable)

- Existing architecture
- Known bottlenecks
- Technical debt
- Deployment constraints

---

## 6. Proposed Architecture

### Architecture Overview
Describe system components and responsibilities.

### System Boundaries
Clarify what is inside vs outside scope.

### Data Model
- Entities
- Relationships
- Partitioning strategy
- Retention and lifecycle policies

### API / Contracts
- Endpoints
- AuthN/AuthZ model
- Versioning strategy
- Backward compatibility considerations

### Workflow
Step-by-step flow including async or failure paths.

---

## 7. Failure Modes & Mitigations

For each critical failure mode:

- Failure scenario
- Detection mechanism
- Mitigation strategy
- Recovery procedure

---

## 8. Security & Privacy Considerations

- Threat model highlights
- Tenant isolation implications
- PII handling
- Encryption (in transit / at rest)
- Secrets management
- Auditability
- Data retention controls

---

## 9. Observability

- Logging strategy
- Metrics
- Tracing
- Alerting
- Health checks
- Success validation signals

---

## 10. Rollout & Migration Plan

- Feature flags
- Canary / blue-green strategy
- Backfill or migration steps
- Backout plan

---

## 11. Testing Strategy

- Unit tests
- Integration tests
- End-to-end tests
- Load / performance testing
- Security testing
- Regression validation

---

## 12. Trade-offs & Alternatives Considered

Summarize key architectural trade-offs and why this design was selected.

---

## 13. Validation & Success Criteria

How will we know this design is working?

- Metrics
- Operational signals
- Review checkpoints

---

## 14. Open Questions

- …

---

## 15. Appendix

- Diagrams
- References
- Linked ADRs

