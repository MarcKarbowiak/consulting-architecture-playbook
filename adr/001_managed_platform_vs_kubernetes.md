# Managed Platform (App Service + Functions) vs Kubernetes

- **Status:** Accepted  
- **Date:** 2026-02-16  
- **Decision Drivers:** Delivery velocity, operational simplicity, security posture, cost control, small team size

---

## Context

We need to deliver a production-ready SaaS platform supporting:

- Request/response APIs
- Asynchronous background processing
- Multi-tenant data isolation
- CI/CD-driven iterative releases

Constraints:

- Small engineering team
- No immediate multi-cloud requirement
- Desire to minimize infrastructure management overhead
- Security expectations aligned with enterprise SaaS norms

The architectural decision: deploy on Kubernetes or use managed Azure platform services.

---

## Decision

Use Azure App Service for API workloads and Azure Functions for asynchronous processing instead of Kubernetes.

---

## Rationale

Managed services provide:

- Reduced operational complexity (no cluster lifecycle management)
- Independent scaling of API and async workloads
- Faster time to production
- Native integration with Azure identity and networking
- Lower cognitive load for a small team

The architecture remains evolvable if orchestration needs increase.

---

## Alternatives Considered

### Kubernetes (AKS)

**Pros:**
- High portability
- Advanced orchestration
- Fine-grained networking control

**Cons:**
- Cluster lifecycle overhead
- Increased observability and security configuration complexity
- Slower initial delivery

**Reason not chosen:** Complexity not justified by current constraints.

### Virtual Machines

**Pros:** Full runtime control  
**Cons:** Manual patching, limited elasticity, higher ops burden

**Reason not chosen:** Not aligned with cloud-native objectives.

---

## Security & Compliance Considerations

Managed platform services:

- Provide managed OS patching
- Reduce control-plane exposure
- Integrate with Managed Identity
- Lower misconfiguration surface

Kubernetes would require:

- Cluster hardening
- Network policies
- RBAC tuning
- Ongoing vulnerability management

Given current team size and compliance requirements, managed services reduce operational security risk.

---

## Trade-offs / Consequences

### Benefits
- Faster delivery
- Lower infrastructure burden
- Simpler deployment model

### Costs / Risks
- Reduced portability
- Less advanced orchestration flexibility

### Mitigations
- Maintain modular architecture
- Avoid unnecessary platform lock-in patterns

---

## Validation / Revisit Criteria

Reassess if:

- Independent services require separate scaling domains
- Multi-cloud becomes contractual
- Advanced networking/orchestration is required

---

## Outcome

The managed platform approach balances security, velocity, and scalability without premature infrastructure complexity.

