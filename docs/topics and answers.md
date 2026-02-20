# Repository Interview Question Bank

## Topics

- Architecture & system design
- Azure platform strategy (managed services vs Kubernetes)
- Multi-tenant isolation & data access boundaries
- Data modeling & partitioning (document DB vs relational)
- Reliability engineering (idempotency, retries, state machines)
- DevSecOps & supply chain controls
- Observability, auditability, and cost telemetry
- Progressive delivery, rollout, and migration discipline
- AI governance for RAG systems (validation, drift, PII)
- Consulting leadership & executive framing

---

## 1. Architecture & System Design

1. The repository positions itself as a “living reference” for repeatable consulting decisions. How do you decide which decisions deserve an ADR versus staying implicit in a design doc, and what is your threshold for “irreversible or costly-to-reverse” decisions?
2. In the “Managed Platform vs Kubernetes” ADR, you explicitly optimize for small-team velocity and security posture. What are the top 3 failure modes you’re *accepting* by choosing managed platform services, and how would you detect each before it becomes an outage or security incident?
3. The “Modular Monolith First” ADR argues for fewer network boundaries to reduce attack surface. What specific module boundary enforcement mechanisms would you use in code and build tooling to prevent the “big ball of mud” outcome?
4. The MeetingAware case study states “asynchronous processing boundary established before scale pressure.” What concrete boundary definition do you use (queues/topics, durable function orchestration, outbox pattern, etc.), and how do you guarantee that boundary doesn’t leak tenant context?
5. In a multi-tenant SaaS, you propose shared infra by default with optional dedicated environments. What are your *quantitative* triggers for promoting a tenant to dedicated infrastructure (data volume, P99 latency, compliance class, RU burn, incident rate), and how do you avoid turning this into an operational tax?
6. The documents emphasize “start simple, scale deliberately.” Describe a staged evolution path from modular monolith on managed platform to more distributed services *without* rewriting core domains. What is the sequencing and what artifacts prove readiness at each step?
7. ADRs include “revisit criteria.” What governance mechanism ensures revisit criteria are actually checked (calendar-based reviews, metric-based alerts, architecture council gates), and what evidence would you want to see in this repo to prove it’s operationalized?
8. MeetingAware uses hybrid search (vector + keyword). How do you design system boundaries so retrieval improvements don’t force coupled changes to extraction prompts/schemas and downstream consumers?
9. Compare the reliability posture of the distributed synchronization case study (explicit message states, dead-lettering, idempotency) to the AI SaaS pipeline (chunking, map-reduce, validation harness). What invariants are shared, and where do the invariants diverge due to probabilistic outputs?
10. The playbook stresses “failure modes as first-class concerns.” Pick one ADR and propose the missing explicit failure-mode table you’d add (scenario, blast radius, detection, mitigation, recovery time), and explain why it wasn’t already included.
11. In MeetingAware, you store raw transcripts separately from structured outputs. What is your boundary for “source of truth,” and how do you handle reprocessing/reconciliation when your extraction schema evolves?
12. In the field operations modernization case, you moved from PDFs to structured capture. What is your strategy for versioning forms/schemas so that old data stays queryable and correct while new validation rules roll out?
13. For YogaFlow’s offline-first constraints, what consistency model did you implicitly choose (read-your-writes, eventual consistency across devices, single-writer), and what failure scenarios does that simplify versus complicate?
14. The repository favors Azure managed identity patterns. How do you model and review trust boundaries between App Service, Functions, storage, and data stores so that identity permissions remain least-privilege over time?
15. For the “tenant context required in every authenticated request” decision, where do you enforce it (gateway, API middleware, data access layer, DB-level constraints), and why is your chosen enforcement point the strongest?
16. How do you ensure the ADR outcomes remain consistent with later templates (Technical Design Doc rollout plan, testing strategy, observability) when multiple engagements reuse the templates?
17. If you had to support a regulated tenant requiring strict data residency and audit trails, which parts of the current “hybrid isolation model” break first, and how would you redesign with minimal disruption?
18. The repo presents separate artifacts (ADRs, case studies, templates). What is the “single narrative” tying them together for a stakeholder, and how do you prevent contradictions as the repository evolves?
19. What assumptions in these ADRs are most likely to be invalidated by business reality (org structure, procurement, compliance), and how do you design decisions to degrade gracefully under those invalidations?
20. If this playbook were adopted by a team of 40 engineers across 5 squads, what changes would you make to the ADR process, module boundaries, and deployment model to preserve velocity?

## 2. Technology & Tooling Choices

1. The ADR picks App Service + Functions instead of AKS. What workloads or operational requirements would force you to reverse that decision, and what “migration readiness” work would you do up front to make reversal non-catastrophic?
2. Cosmos DB is chosen for “schema evolution” and tenant-partitioned access patterns. What would make you choose Postgres/SQL Server instead, and how would that change your tenant isolation model and scaling plan?
3. MeetingAware includes Azure AI Search (vector + hybrid) plus Azure OpenAI. What are the main failure and cost characteristics of each dependency, and how do you prevent any single one from becoming a systemic bottleneck?
4. When you choose hybrid search, what is your strategy for relevance regression testing and “golden set” maintenance as embeddings/models change?
5. The distributed sync platform used WCF + SQL queue + Oracle (legacy constraints). If you were modernizing today, what tool choices would you make for messaging and why (managed broker, CDC, event streaming), and what invariants must remain unchanged?
6. In the YogaFlow case, Cordova + SQLite was chosen for simplicity. If you were building today, what would you replace (if anything) and what tradeoffs would you refuse because of the business constraints?
7. This repo references Azure DevOps CI/CD in case studies but contains no pipeline definitions. What’s the minimum set of pipeline-as-code artifacts you expect in a “consulting-grade” repo, and why?
8. For feature flags mentioned in templates/case study refinement notes, which flagging approach would you choose (custom, managed service) and what governance requirements would you impose (expiry, ownership, audit)?
9. The ADRs emphasize security posture. What security tooling would you baseline (SAST, dependency scanning, secret scanning, IaC scanning) and how do you keep signal-to-noise manageable for small teams?
10. If you needed cross-cloud portability later, what are the top three Azure-specific choices here that create lock-in, and which ones are “acceptable lock-in” versus “dangerous lock-in”?
11. The MeetingAware case mentions chunking + map-reduce to manage token limits. What orchestration/tooling choice best fits the reliability and traceability needs, and what tradeoff are you making in developer ergonomics?
12. What documentation tooling (docs-as-code, linting, link checking) would you add to ensure these Markdown artifacts stay consistent and don’t rot?

## 3. Code Structure & Implementation Patterns

1. The tenant isolation ADR depends on “TenantId in every authenticated request.” Sketch the enforcement chain in code: where does TenantId come from, how is it validated, and how do you prevent “confused deputy” attacks where a caller supplies a different TenantId?
2. For Cosmos DB access, what data access abstraction do you use to guarantee that every query is tenant-scoped, and how would you make it difficult for an engineer to accidentally run cross-partition queries?
3. How do you structure a modular monolith in Node.js/C# so that modules have clear public interfaces (APIs) and private internals? What’s your mechanism for preventing “internal” code from being imported across boundaries?
4. MeetingAware uses asynchronous processing for ingestion/chunking/extraction. What is your idempotency key strategy across the pipeline, and how do you handle “exactly-once illusion” at-least-once execution semantics?
5. The distributed sync case study introduces explicit message states. What state machine design do you prefer (enum + transitions, table-driven, workflow engine), and how do you ensure transitions are auditable and testable?
6. The case studies mention “structured output schemas” for AI extraction. How do you version these schemas, enforce them at runtime, and handle partial/invalid outputs without silently degrading product behavior?
7. For the PII redaction pipeline (optional), where do you place it in the ingestion flow to minimize data exposure while preserving model quality, and how do you design for reprocessing when redaction logic changes?
8. The templates call out “rollout & migration plan” (feature flags, canary, backout). What code-level patterns do you use to make backouts safe (expand/contract DB changes, backward-compatible contracts, dual writes), and what do you refuse to merge without them?
9. How do you implement cost telemetry “around embeddings” so it can be attributed per tenant, per feature, and per request path without leaking sensitive content into logs?
10. For offline-first (YogaFlow), describe how you implement transactional credit updates and invariants in SQLite so that a partially completed operation cannot corrupt balances.
11. What are the most important “guardrail tests” you’d add to prevent cross-tenant leakage (unit, integration, property-based, contract tests), and where do they run in CI?
12. The repo stresses “avoid copying blindly.” If you were turning this playbook into a generator/scaffold (templates + conventions), what code would you write to enforce consistency and what would you intentionally leave manual?

## 4. DevSecOps & Supply Chain Controls

1. This repository has no explicit CI/CD definitions or security scanning configs. If you were reviewing a candidate’s repo, what questions do you ask to determine whether security maturity is real versus implied?
2. What is your minimum viable supply chain posture for a small team shipping to production weekly (dependency pinning, lockfiles, artifact signing, provenance, SBOM), and which parts are “must have now” versus “add later”?
3. How would you implement secret management for App Service + Functions + pipelines, and how do you prevent secrets from leaking via logs, crash dumps, or misconfigured app settings?
4. For multi-tenant systems using TenantId partitioning, what are your threat model top items (data exfiltration, broken authZ, query injection patterns, insider risk), and what controls map to each?
5. What deployment permissions model do you enforce (environments, approvals, separation of duties), and how do you balance speed with compliance when you have enterprise customers?
6. How do you handle vulnerability management for managed services (where patching is “managed”) versus your own app code and dependencies (which are not)?
7. What is your strategy for auditing access to production data (including developers), and what evidence would you expect to see in logs and access reviews?
8. In the AI SaaS scenario, what new supply chain risks appear (prompt injection, data poisoning, model configuration drift), and how do you integrate controls into CI/CD?
9. How do you ensure infrastructure and configuration drift are detectable if you are not using IaC in-repo? What’s your acceptable drift risk threshold?
10. Describe your incident response and postmortem expectations for security incidents involving potential cross-tenant leakage: containment, notification, evidence collection, and remediation steps.
11. How do you prevent “shadow endpoints” and “debug features” from reaching production (build flags, environment config, runtime checks), especially in a modular monolith?
12. What is your approach to compliance readiness artifacts (data retention policy, DPIA/PIA, threat model, audit logs) for a product like MeetingAware handling PII-heavy transcripts?

## 5. Reliability & Testing Strategy

1. The MeetingAware case study mentions a regression validation pipeline for AI outputs. What does “regression” mean in probabilistic systems, and what metrics and test sets do you choose to make it actionable?
2. How do you design E2E tests for tenant isolation where the primary risk is accidental cross-tenant retrieval through search indexes and caches?
3. In the distributed sync system, you added dead-lettering and retries. What are your retry policies (max attempts, backoff, jitter), and how do you avoid retry storms when downstream Oracle is degraded?
4. The templates call for load/performance testing. For Cosmos DB partitioning by TenantId, what performance tests would you run to detect hot partitions and RU exhaustion before production?
5. What is your reliability strategy for asynchronous pipelines (Functions) under partial failures (storage transient errors, AI provider throttling, search indexing delays)? How do you choose between “fail fast” vs “degrade gracefully”?
6. How do you test chunking and map-reduce extraction so that token-limit handling doesn’t silently drop or duplicate content?
7. In the field operations modernization case, what tests ensure schema versioning doesn’t break older clients during incremental rollout?
8. For offline-first YogaFlow, what is your backup/restore test plan to prove that periodic backups actually work when a device fails?
9. What are your top 5 reliability signals (SLIs) for MeetingAware (ingestion success rate, extraction accuracy proxy, indexing latency, tenant boundary violations, cost per transcript), and which are release blockers?
10. Describe a chaos/failure injection experiment you would run for each case study system (AI SaaS, sync platform, offline-first app) to validate the stated mitigations.
11. If a tenant triggers a “hot partition” incident, what is your playbook: detection, immediate mitigation, longer-term re-partitioning strategy, and customer communication?
12. What is your testing stance on “AI correctness”: when do you require human review, and when do you allow fully automated extraction outputs to drive workflows?

## 6. Observability & Telemetry

1. The documents call for “operational telemetry” and “cost telemetry around embeddings.” What is your telemetry model (logs/metrics/traces) and correlation strategy across App Service, Functions, Cosmos DB, storage, search, and AI calls?
2. What is your policy for logging sensitive content (transcripts, extracted fields, PII) while still enabling debugging and auditability?
3. How do you detect cross-tenant leakage attempts: what events do you log, what alerts do you set, and what “near-miss” signals matter (blocked attempts, unusual query patterns)?
4. What dashboards would you build for the distributed sync platform to reduce manual intervention (backlog size, state transition rates, DLQ volume, processing latency), and what thresholds indicate imminent failure?
5. In MeetingAware, retrieval misses are a key failure mode. What observability would you add to measure retrieval quality continuously (query-to-hit ratio, recall proxies, human feedback loops)?
6. How do you instrument the AI validation harness so that it can be used for release gating as well as production drift detection?
7. For a modular monolith, what’s your strategy for “logical service boundaries” in tracing so that module-level latency and errors are visible without splitting processes?
8. How do you attribute cost and performance per tenant without creating cardinality explosions in metrics systems?
9. What is your approach to sampling and redaction in distributed traces that may contain sensitive payloads?
10. What operational runbooks or on-call documentation would you expect alongside these templates, and what’s missing today?

## 7. Progressive Delivery & Deployment Model

1. The case studies mention CI/CD discipline and incremental rollout. What deployment patterns do you standardize for small teams (blue/green, canary, ring deployments), and what evidence would you want to see in this repo?
2. The technical design template includes backout plans. What are the practical constraints that make backout impossible (schema changes, irreversible side effects), and how do you design to keep backout feasible?
3. How do you use feature flags to decouple deployment from release without accumulating permanent complexity (expiry, ownership, test coverage, kill switches)?
4. In a multi-tenant system, how do you roll out changes tenant-by-tenant safely (opt-in tenants, staged rollout, per-tenant configuration), and what do you do when a single tenant needs rollback?
5. For AI models/prompts/schema evolution, what is your progressive delivery model (A/B, shadow mode, offline evaluation gates), and what’s your rollback mechanism when quality regresses?
6. How do you handle data migrations in Cosmos DB while maintaining tenant isolation guarantees and minimizing RU spikes?
7. Describe a deployment pipeline policy that prevents “unsafe” changes from shipping (e.g., cross-partition query introduction, removal of tenant filter) and how you enforce it (lint rules, tests, code owners).
8. If you introduce dedicated environments for regulated tenants, how do you handle version skew between shared and dedicated deployments, and what governance prevents drift?
9. What is your plan for disaster recovery and region failover for MeetingAware, and how does your deployment model validate DR readiness (game days, automated tests)?
10. How do you ensure your delivery model scales when you move from “small team” to “multiple teams with isolated pipelines” (a revisit criterion in the modular monolith ADR)?

## 8. Governance & Risk (AI-specific if relevant)

1. The repository frames AI outputs as “probabilistic and testable.” What governance structure do you put in place so that product decisions aren’t made on vibes (model change control, evaluation gates, sign-offs, audit trails)?
2. MeetingAware handles PII-heavy transcripts. What is your data retention and deletion model (including backups and search indexes), and how do you prove deletion actually propagates?
3. The case study mentions “optional PII redaction.” What makes redaction optional versus mandatory, and how do you prevent “optional” from becoming “never enabled” for high-risk tenants?
4. What is your stance on storing prompts, model parameters, and evaluation results for auditability, and where does that live (config repo, DB, artifact store)?
5. How do you manage prompt injection and data exfiltration risks in a RAG system (input validation, retrieval filtering, content safety policies, isolation by tenant)?
6. How do you implement tenant-scoped governance for AI: per-tenant model selection, per-tenant evaluation thresholds, and per-tenant opt-out of data usage?
7. What is your policy for human-in-the-loop review: which outputs require review, how do you sample, and how do you prevent review processes from becoming a bottleneck?
8. How do you define and measure “hallucination” in a way that’s meaningful for compliance workflows, and what actions do you take when hallucination scoring degrades?
9. The ADRs include “security and compliance considerations,” but there is no explicit threat model artifact. What would your threat model cover for MeetingAware, and how do you keep it updated as features evolve?
10. How do you handle customer-facing incident communication when AI extraction is wrong but not “down” (silent quality failures)? What’s your governance for deciding when to notify?

## 9. Scalability & Performance

1. Scenario: One tenant becomes 50× larger than all others and drives hot partitions in Cosmos DB. What are your immediate mitigations and longer-term redesign options (hierarchical partition keys, per-tenant containers, dedicated environment), and what are the tradeoffs?
2. Scenario: Azure OpenAI starts throttling intermittently during peak hours. How do you maintain ingestion SLAs (queueing, backpressure, prioritization, partial processing) without exploding cost?
3. Scenario: Search indexing lags by 30 minutes during a batch import. What product behaviors change (freshness, compliance workflows), and how do you design the system to make lag visible and safe?
4. Scenario: A cross-tenant leakage bug is found in a retrieval query. How do you scope impact quickly (logs, audit trails), contain, and prevent recurrence (tests, guardrails, code structure)?
5. Scenario: The modular monolith hits scaling limits in one module (e.g., ingestion). How do you extract just that capability into a service while preserving data integrity and shared authZ?
6. Scenario: Chunking strategy causes token usage to spike 3×. How do you find root cause (prompt changes, transcript length, retrieval size), and what controls prevent future regressions?
7. Scenario: Field operations app adoption grows from 750 users to 10,000 across regions. What are your scaling concerns (rate limits, partition strategy, offline usage, RBAC) and what do you change first?
8. Scenario: Distributed sync backlog grows for 8 hours due to downstream DB slowness. How do you stabilize throughput (bounded concurrency tuning, prioritization) and prevent data mismatch?
9. Scenario: Offline-first YogaFlow expands to multiple locations wanting optional sync. What is the minimal sync design that preserves simplicity while preventing double-spend of credits?
10. Scenario: A regulated tenant requires customer-managed keys and strict audit logs. What parts of the platform need redesign and what performance impacts do you anticipate?

## 10. Cost & Operational Tradeoffs

1. In MeetingAware, what is your cost model per transcript (storage, search indexing, embedding, LLM calls), and which levers reduce cost without harming quality (chunk size, caching, hybrid retrieval tuning)?
2. For managed services vs AKS, show how you would present a 12-month TCO comparison that accounts for headcount and operational risk, not just cloud bill.
3. Cosmos DB cost is often RU-driven. What practices do you use to prevent runaway RU spend (query discipline, indexing policy, TTL/retention, request shaping), and what governance enforces them?
4. What are the hidden operational costs of offering dedicated environments per tenant (pipelines, monitoring, incident response), and how do you price/justify them?
5. In the distributed sync platform, what was the cost of *not* having proper observability (manual intervention, data mismatches), and how do you quantify the ROI of reliability work?
6. For offline-first YogaFlow, what is your operational risk cost (device loss, manual backups), and what minimal investment yields the biggest risk reduction?
7. How do you set budgets and alerts per tenant or per feature so that “one tenant’s behavior” can’t surprise your finance model?
8. If you had to cut cloud spend by 30% in 60 days, what do you change first in the MeetingAware architecture without violating tenant isolation and compliance constraints?

## 11. Consulting & Leadership Framing

1. Explain the “managed platform vs Kubernetes” decision to a skeptical CTO who believes “real companies run Kubernetes.” What do you say, and what evidence do you bring?
2. Explain the “document DB vs relational” decision to a data team that wants SQL joins for analytics. How do you negotiate a solution that meets both product velocity and reporting needs?
3. Present the tenant isolation strategy to a compliance officer: what guarantees are provided, what are the known risks, and what controls reduce those risks?
4. You’re asked to justify “modular monolith first” to a VP Engineering who wants microservices for org design reasons. How do you frame the sequencing and avoid ideological debate?
5. Describe how you run a discovery workshop to surface constraints that would invalidate your default patterns (e.g., data residency, integration complexity, on-prem requirements).
6. A client asks for “AI insights” with no governance budget. How do you set expectations, define acceptable risk, and decide what you will not build?
7. How do you measure success of your architecture work across engagements: what outcomes and signals matter beyond “we shipped”?
8. Describe a time you had to reverse an architectural decision. What triggered it, what evidence supported reversal, and how did you communicate it without losing stakeholder trust?
9. How do you create and maintain alignment between product, engineering, security, and operations when shipping quickly in a small team?
10. What is your strategy for mentoring teams to use ADRs and templates without creating bureaucratic drag?

## 12. Hard Panel Questions

1. These ADRs are dated 2026-02-16 and look internally consistent—but where is the evidence that they were tested in production (metrics, incident learnings, postmortems) rather than written as “ideal architecture”?
2. You claim TenantId partitioning “enforces isolation,” but partitioning alone doesn’t prevent a bad query from reading across tenants. What is the strongest *technical* control you would implement to make cross-tenant reads impossible, and why isn’t it described here?
3. You argue managed services reduce misconfiguration risk. Managed services also hide complexity. Describe a concrete incident where the “hidden complexity” of managed services harmed you, and how you mitigated it.
4. A modular monolith can become unmaintainable fast. What would convince you you’re already too late (code ownership signals, dependency graph, build times), and what would you do in the next 30 days to stop the bleeding?
5. In MeetingAware, you rely on Azure OpenAI and AI Search. What is your plan when either service has a prolonged regional outage? “Wait it out” is not an acceptable answer.
6. Your AI regression validation pipeline is mentioned but not specified. What are the acceptance thresholds, who owns them, and how do you prevent teams from lowering thresholds under delivery pressure?
7. You mention “optional PII redaction.” If you ship without it for a tenant that later demands it, how do you remediate historical stored content and prove compliance retroactively?
8. The repo calls out “feature-flag rollout controls” as a refinement. Why weren’t flags part of the initial design given the emphasis on controlled iteration, and what risk did that create?
9. The sync platform improvements (idempotency, DLQ, explicit state) are table-stakes reliability. Why did the original system ship without them, and what does that say about engineering governance?
10. This is a playbook repo with templates, but no examples of completed design docs using the templates. How do you prove these templates are actually usable under time pressure and not just aspirational?

---

# Learning Gap Signals

- Topics the candidate must deeply understand to answer well
  - Multi-tenant security boundaries and defense-in-depth (authN/authZ, tenant context enforcement, auditability)
  - Partitioning and access-pattern-driven data modeling (including hot partition mitigation and query discipline)
  - Async reliability patterns (idempotency keys, retries/backoff, dead-lettering, state machines, backpressure)
  - Progressive delivery mechanics (feature flags governance, safe rollback, schema evolution strategies)
  - AI system governance for RAG (evaluation design, drift detection, PII handling, prompt/model/version control)
  - Observability design (correlation, sensitive-data-safe telemetry, cost attribution)

- Areas that appear weak or under-explained
  - Concrete implementation details for tenant enforcement (where/how it is guaranteed across API, data, and search)
  - Specifics of the AI regression validation pipeline (datasets, metrics, gates, ownership)
  - Operational processes that make “revisit criteria” real (cadence, decision review, enforcement)

- Areas that could trigger skepticism
  - CI/CD and security maturity is asserted in narratives but not evidenced by pipeline-as-code or scanning configs
  - “Partition key = isolation” can be misunderstood; strong isolation requires layered controls beyond partitioning
  - Managed-services-first choices can mask vendor dependency risks unless DR/failover is explicitly designed

- Missing artifacts (e.g., ADRs, SLOs, threat model, rollback strategy)
  - CI/CD pipeline definitions and environment promotion/approval model
  - Test strategy evidence (test harnesses, quality gates, load testing results)
  - Threat model(s) and data protection/retention policy artifacts (especially for PII-heavy AI workloads)
  - SLO/SLI definitions, alerting standards, runbooks, and incident/postmortem examples
  - Infrastructure-as-Code and configuration baselines (or an explicit rationale for their absence)
