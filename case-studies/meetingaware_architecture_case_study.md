# Case Study: MeetingAware — Secure Multi-Tenant AI SaaS Platform

## 1. Executive Summary

MeetingAware is a multi-tenant SaaS platform that transforms unstructured meeting transcripts into structured, compliance-ready insights using Retrieval-Augmented Generation (RAG) and large language models. The architecture prioritizes tenant isolation, secure-by-design cloud patterns, and disciplined AI validation.

---

## 2. Business Context & Constraints

### Problem
Organizations generate high volumes of meeting data containing decisions, risks, and requirements that are rarely structured or searchable.

### Objectives
- Extract structured signals from transcripts
- Support compliance and audit workflows
- Enable AI-driven insights
- Operate as a scalable SaaS platform

### Constraints
- Sensitive PII in transcripts
- Small engineering team
- Rapidly evolving product requirements
- Need for fast iteration without compromising governance

---

## 3. My Role & Scope

- Defined overall system architecture and tenant isolation model
- Implemented core ingestion, chunking, and extraction pipelines
- Designed structured output schemas for signal extraction
- Built regression validation pipeline for AI outputs
- Established CI/CD delivery discipline
- Worked directly with early users to refine signal models

---

## 4. High-Level Architecture

### Core Stack
- React frontend
- Node.js API (Azure App Service)
- Azure Functions (async processing)
- Azure Cosmos DB (document model)
- Azure Blob Storage
- Azure AI Search (vector + hybrid)
- Azure OpenAI
- Azure DevOps CI/CD

### Architectural Pattern
Modular monolith with asynchronous processing extensions.

---

## 5. Foundational Decisions Made Early

- TenantId used as partition key from day one
- Async processing boundary established before scale pressure
- Structured outputs separated from raw transcripts
- Managed services preferred over Kubernetes to reduce operational load
- Regression validation integrated early to reduce AI risk

---

## 6. Key Architectural Decisions & Trade-offs

- Managed services over container orchestration for speed and simplicity
- Document DB over relational to accommodate evolving AI schemas
- Hybrid search (vector + keyword) to reduce retrieval misses
- Modular monolith to avoid premature microservices

---

## 7. Failure Modes & Mitigations

- Token limits → chunking + map-reduce strategy
- Retrieval misses → hybrid search + validation harness
- Cross-tenant leakage risk → enforced TenantId scoping
- Hot partitions → monitoring + isolation strategy

---

## 8. Security & Operational Considerations

- Managed identity between services
- Tenant-boundary enforcement at API layer
- Optional PII redaction pipeline
- Controlled transcript lifecycle
- Evaluation harness to detect extraction drift

---

## 9. What I Would Refine Today

- Introduce more granular cost telemetry around embeddings
- Further isolate indexing workloads for large tenants
- Formalize model comparison testing across deployments
- Add structured hallucination scoring

---

## 10. Outcomes & Lessons

- Demonstrated pragmatic AI-enabled SaaS architecture
- Established governance patterns suitable for client environments
- Balanced velocity and security within a small-team constraint
- Reinforced importance of treating AI systems as testable infrastructure

This system reflects a greenfield foundation approach: start simple, make boundaries explicit, and evolve deliberately.

