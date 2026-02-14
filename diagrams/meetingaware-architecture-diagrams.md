# MeetingAware Architecture Diagrams (Mermaid)

Copy this file into:

- `diagrams/meetingaware-architecture-diagrams.md`

These diagrams are written in **Mermaid** and simplified to avoid common GitHub rendering issues (no multiline labels, minimal special characters).

---

## 1) Context Diagram (System Landscape)

```mermaid
flowchart LR

  User[User / Customer]
  WebApp[Web App - React]
  API[Backend API - Node.js - Azure App Service]
  Cosmos[Azure Cosmos DB]
  Blob[Azure Blob Storage]
  Functions[Azure Functions - Async Processing]
  AISearch[Azure AI Search - Vector Hybrid]
  OpenAI[Azure OpenAI]
  ManagedId[Managed Identity]

  User --> WebApp
  WebApp --> API

  API --> Cosmos
  API --> Blob
  API --> Functions

  Functions --> Blob
  Functions --> Cosmos
  Functions --> AISearch
  Functions --> OpenAI

  API -.auth.-> ManagedId
  Functions -.auth.-> ManagedId
  ManagedId -.access.-> Cosmos
  ManagedId -.access.-> Blob
  ManagedId -.access.-> AISearch
  ManagedId -.access.-> OpenAI
```

---

## 2) Container Diagram (Internal Components)

```mermaid
flowchart TB

  FE[React Web App]
  API[Node API - App Service]
  Auth[Auth Middleware - Tenant Context]

  Ingest[Ingestion Orchestrator]
  Redact[PII Redaction]
  Chunk[Chunking and Metadata]
  Index[Index to AI Search]
  Extract[LLM Extraction]
  Validate[Evaluation and Regression]
  Store[Persist Structured Results]

  Cosmos[Cosmos DB - Tenant Partition]
  Blob[Blob Storage - Tenant Scoped]
  AISearch[AI Search]
  OpenAI[Azure OpenAI]

  FE --> API
  API --> Auth
  API --> Cosmos
  API --> Blob
  API --> Ingest

  Ingest --> Redact
  Redact --> Chunk
  Chunk --> Index
  Index --> Extract
  Extract --> Validate
  Validate --> Store

  Ingest --> Blob
  Store --> Cosmos

  Index --> AISearch
  Extract --> AISearch
  Extract --> OpenAI
```

---

## Notes

- TenantId is enforced in the API layer and used as the Cosmos partition key.
- API and Functions scale independently.
- Validation stage protects against RAG regressions and schema drift.

These diagrams intentionally favor clarity over visual complexity for consultancy discussions.

