# Discovery Workshop Template

A structured facilitation guide for client-facing technical discovery sessions in boutique consultancy environments.

This template is designed to:

- Reduce ambiguity early
- Surface hidden constraints and risks
- Align business goals with technical strategy
- Produce actionable architectural direction

---

# 1. Workshop Overview

## Objective
Clearly state the purpose of the session.

## Success Criteria
What must be clarified by the end?

- Defined problem scope
- Identified system boundaries
- Agreed non-functional priorities
- Clear architectural direction

---

# 2. Business Context & Drivers

## Business Goals
- Revenue growth
- Cost reduction
- Risk mitigation
- Regulatory compliance
- Time-to-market acceleration

## Stakeholders
- Executive sponsor
- Product owner
- Engineering lead
- Security / compliance representative

## Key Metrics
What does success look like?

- Growth targets
- Latency or availability targets
- Deployment frequency
- Cost ceilings

---

# 3. Current State Assessment

## Existing Architecture
- Monolith or distributed
- On-prem or cloud
- Vendor dependencies

## Pain Points
- Reliability issues
- Performance bottlenecks
- Deployment friction
- Developer productivity constraints

## Technical Debt
- Outdated frameworks
- Manual processes
- Missing observability

---

# 4. Functional Requirements

## Core User Flows
Document primary workflows step-by-step.

## Edge Cases
- Large inputs
- Partial failures
- Permission constraints

---

# 5. Non-Functional Requirements

## Scalability
- Current vs projected load

## Availability
- Uptime requirements
- Downtime impact

## Security & Compliance
- PII present?
- Regulatory requirements?
- Data residency constraints?

## Performance
- Response time targets
- Batch vs real-time

## Cost Constraints
- Budget limits
- Cost predictability

---

# 6. Integration Landscape

## External Systems
- APIs
- Legacy databases
- Identity providers
- Payment providers

## Data Flow
Sketch high-level data movement.

---

# 7. Architectural Hypotheses

Propose and test early architecture directions.

Examples:
- Modular monolith vs microservices
- Managed services vs container orchestration
- NoSQL vs relational
- Event-driven vs synchronous APIs

Document risks and unknowns.

---

# 8. Risk Identification

## Technical Risks
- Scaling uncertainty
- Vendor lock-in
- Data model instability

## Organizational Risks
- Skill gaps
- Change resistance
- Timeline pressure

## Mitigation Ideas
- Proof-of-concept spikes
- Load testing experiments
- Data modeling validation

---

# 9. Early Unlocks

Identify 1–3 high-impact actions that reduce uncertainty quickly.

---

# 10. Output Artifacts

At workshop conclusion, produce:

- Architecture summary (1–2 pages)
- Identified trade-offs
- Risk register
- High-level roadmap
- Open questions list

---

# Facilitation Principles

- Tie technical decisions to business drivers
- Avoid premature over-engineering
- Make trade-offs explicit
- Capture assumptions clearly
- End with defined next steps and ownership

