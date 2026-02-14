# Case Study: MeetingAware — Designing a Secure Multi‑Tenant AI SaaS Platform on Azure

## Executive Summary

MeetingAware is a multi-tenant SaaS platform that transforms unstructured meeting transcripts (and optionally audio) into structured, compliance-ready outputs using Retrieval-Augmented Generation (RAG) and large language models.

The system was designed to prioritize:

- Secure-by-design cloud architecture
- Tenant isolation and data lifecycle control
- Pragmatic delivery velocity
- AI output consistency and regression safety
- Scalable SaaS foundations without premature complexity

This case study outlines the architectural decisions, trade-offs, failure modes, and scaling strategy behind the platform.

---

# 1. Business Context

## Problem
Organizations conduct high volumes of meetings containing decisions, risks, requirements, and compliance-relevant information. These insights are typically lost in unstructured transcripts.

## Objectives

- Extract structured signals from meeting content
- Support compliance-oriented workflows (e.g., documentation, audit trails)
- Enable AI-powered insights while maintaining privacy controls
- Provide a scalable SaaS model for multiple tenants

## Constraints

- Sensitive data (PII present in transcripts)
- Evolving product requirements and data schema
- Limited engineering capacity (small team)
- Need for fast iteration without heavy platform overhead

---

# 2. High-Level Architecture

## Core Components

- **Frontend:** React-based web application
- **API Layer:** Node.js backend hosted on Azure App Service
- **Async Processing:** Azure Functions for analysis and background workflows
- **Data Store:** Azure Cosmos DB (document model)
- **Object Storage:** Azure Blob Storage
- **Vector Search:** Azure AI Search
- **LLM Integration:** Azure OpenAI
- **CI/CD:** Azure DevOps pipelines with progressive delivery patterns

## Architectural Pattern

A **modular monolith with async extensions** was chosen over microservices or Kubernetes orchestration.

Rationale:
- Reduced operational overhead
- Clear separation of request/response vs background workloads
- Independent scaling of API and analysis workloads
- Faster delivery with manageable complexity

---

# 3. Key Architectural Decisions

## 3.1 App Service + Functions vs Kubernetes

Kubernetes was evaluated but rejected due to:

- Increased operational complexity (cluster lifecycle, networking, upgrades)
- Limited need for advanced orchestration
- Small team size and delivery velocity goals

Azure App Service + Functions provided:

- Independent scaling
- Reduced infrastructure management
- Faster time to production

Portability trade-off acknowledged. Kubernetes remains a future option if scale or orchestration complexity demands it.

---

## 3.2 Cosmos DB vs Relational Database

Cosmos DB was selected because:

- Document model supports evolving schemas
- TenantId-based partitioning aligns with SaaS access patterns
- Managed service reduces operational overhead

Trade-offs:
- Cross-partition queries must be avoided
- Data modeling discipline required

Relational DB would be preferred if:
- Strong relational integrity or complex transactional workflows dominated

---

## 3.3 Tenant Isolation Strategy

Isolation approach:

- TenantId required on every authenticated request
- Cosmos DB partition key = TenantId
- Blob storage logically segmented by tenant
- Large or regulated tenants may deploy dedicated Azure environments

This hybrid model balances cost efficiency (shared infra) with enterprise-grade isolation when required.

---

# 4. AI & RAG Architecture

## Pipeline Overview

1. Transcript uploaded
2. Optional PII redaction step
3. Chunking and indexing into vector search
4. Context retrieval based on query or signal framework
5. LLM extraction and structured output generation
6. Validation and storage of results

## Design Principles

- Retrieval before generation
- Structured prompt pipelines
- Versioned knowledge base
- Evaluation harness for regression detection

---

# 5. Failure Modes & Mitigations

## 5.1 Token Limit Constraints

Risk: Long transcripts exceed model context window.

Mitigations:
- Chunking strategy (semantic + size limits)
- Map-reduce summarization
- Retrieval-scoped extraction
- Explicit transcript size limits with user feedback


## 5.2 Retrieval Misses

Risk: RAG fails to retrieve relevant context.

Mitigations:
- Hybrid search (vector + keyword)
- Metadata tagging (meeting type, tenant, domain)
- Golden transcript regression suite
- Continuous evaluation pipeline


## 5.3 No Signal Scenario

Risk: Transcript lacks actionable decisions.

Mitigations:
- Explicit “no findings” state
- Avoid hallucinated output
- Suggest facilitation improvements


## 5.4 Hot Partition / Tenant Imbalance

Risk: Large tenant drives disproportionate load.

Mitigations:
- Monitor RU consumption per tenant
- Throttle or isolate high-usage tenants
- Offer dedicated deployments

---

# 6. Security & Privacy Controls

- Managed identity between services
- Controlled transcript lifecycle (upload → processing → deletion)
- Optional redaction pipeline
- Tenant-aware data access enforcement
- Secure-by-default cloud configuration

Security posture prioritized architectural simplicity over excessive distributed complexity.

---

# 7. Delivery & DevOps Practices

- CI/CD with progressive rollout
- Feature flags for AI capability releases
- Regression validation for RAG knowledge base changes
- Delivery telemetry aligned with DORA metrics

Result: Frequent releases with controlled risk exposure.

---

# 8. Scalability Path

Short-term scaling:
- Horizontal scale App Service
- Scale Functions independently
- Optimize partition strategy

Mid-term scaling:
- Dedicated environments for large tenants
- Enhanced observability (OpenTelemetry)

Long-term evolution:
- Selective extraction of bounded contexts into services
- Containerization if orchestration needs increase

---

# 9. Lessons Learned

- Managed cloud services accelerate early delivery
- Tenant partitioning discipline is critical
- AI systems require evaluation harnesses, not just prompts
- Modular monolith reduces premature distributed complexity
- Clear documentation builds stakeholder trust

---

# Summary

MeetingAware demonstrates how to build a secure, multi-tenant AI-enabled SaaS platform using pragmatic architecture principles. The system balances delivery velocity, scalability, and governance while preserving a clear path for future evolution.

The design favors clarity, explicit trade-offs, and operational simplicity — key attributes in boutique consultancy engagements.

