# Discovery Workshop Template

A structured template for running client-facing technical discovery sessions in boutique consultancy environments.

This template is designed to:
- Reduce ambiguity early
- Surface hidden constraints and risks
- Align business goals with technical strategy
- Produce actionable architectural direction

---

# 1. Workshop Overview

## Objective
Clearly state the purpose of the session.

Example:
- Modernize legacy platform
- Define architecture for new SaaS product
- Evaluate scalability bottlenecks
- Assess AI enablement feasibility

## Success Criteria
What must be clarified by the end of the session?

- Defined problem scope
- Identified system boundaries
- Known integration points
- Agreed non-functional priorities
- Clear next-step architecture direction

---

# 2. Business Context & Drivers

## Business Goals
- Revenue growth?
- Cost reduction?
- Risk mitigation?
- Regulatory compliance?
- Time-to-market acceleration?

## Stakeholders
- Executive sponsor
- Product owner
- Engineering lead
- Compliance/security representative

## Key Metrics
What does success look like?
- Latency targets
- User growth expectations
- Deployment frequency
- Operational cost limits

---

# 3. Current State Assessment

## Existing Architecture
- Monolith or distributed?
- On-prem or cloud?
- Vendor dependencies?

## Pain Points
- Performance bottlenecks
- Deployment friction
- Reliability issues
- Developer productivity constraints

## Technical Debt
- Outdated frameworks
- Manual deployment steps
- Missing observability

---

# 4. Functional Requirements

## Core User Flows
Document primary workflows step-by-step.

Example:
1. User uploads document
2. System processes content
3. Insights generated
4. Results exported

## Edge Cases
- Large inputs
- Partial failures
- Permission constraints

---

# 5. Non-Functional Requirements (Critical for Architecture)

## Scalability
- Expected users (now vs 12–24 months)
- Peak load patterns

## Availability
- Required uptime (99%, 99.9%, 99.99%)
- Business impact of downtime

## Security & Compliance
- PII?
- Regulatory requirements?
- Data residency constraints?

## Performance
- Acceptable response time
- Batch vs real-time expectations

## Cost Constraints
- Budget ceiling
- Cost predictability requirements

---

# 6. Integration Landscape

## External Systems
- APIs
- Legacy databases
- Identity providers
- Payment providers

## Data Flow
Sketch high-level data movement.

- Ingestion
- Processing
- Storage
- Output

---

# 7. Architectural Hypotheses

During workshop, propose and test early architecture directions.

Examples:
- Modular monolith vs microservices
- Managed cloud services vs container orchestration
- NoSQL vs relational
- Event-driven processing vs synchronous APIs

Document:
- Why it might work
- Risks
- Unknowns

---

# 8. Risk Identification

## Technical Risks
- Scaling uncertainty
- Vendor lock-in
- Data model instability

## Organizational Risks
- Skill gaps
- Change resistance
- Delivery timeline pressure

## Mitigation Ideas
Outline possible early experiments or proofs-of-concept.

---

# 9. Early Unlocks (High-Impact Next Steps)

Identify 1–3 actions that reduce uncertainty quickly:

- Proof-of-concept for critical path
- Load test small component
- Data modeling spike
- Authentication integration spike

---

# 10. Output Artifacts

At the end of the workshop, produce:

- Architecture summary (1–2 pages)
- Identified trade-offs
- Risk register
- Proposed delivery roadmap (high-level)
- Open questions requiring follow-up

---

# Facilitation Notes (Consulting Perspective)

- Keep architecture discussions tied to business drivers
- Avoid over-engineering during discovery
- Make trade-offs explicit
- Capture assumptions clearly
- Validate understanding before moving forward
- End with clear next steps and ownership

---

# Example Closing Statement

"Based on today’s session, the pragmatic path is to start with a modular cloud-native architecture using managed services to reduce operational overhead. We will validate scalability assumptions early and evolve complexity only when justified by growth or regulatory needs."

