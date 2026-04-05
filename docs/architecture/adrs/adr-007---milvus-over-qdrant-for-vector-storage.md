# ADR-007 - Milvus over Qdrant for vector storage

**Date:** 2026-04-05
**Status:** Accepted
**Decider:** Roberto (Head of AI), Cedric (Lead AI Enterprise Architect)
**Stakeholders:** Platform, Security, Compliance

---

## Principles Driving This Decision

- **P4 - Sovereignty First - UAE PDPL**

## Context

Every Elite Gold client has a profile stored as vector embeddings: transaction history, preferences, risk profile, communication style. These are updated in real time and queried by agents to build context for recommendations. Milvus and Qdrant are both vector databases that can scale to millions of embeddings.

Client embeddings are PII - ARCH-CON-01 requires they stay in UAE. Both Milvus and Qdrant can be self-hosted, but Milvus has better Ceph S3 integration (critical for our distributed storage).

### Applicable Constraints

- **ARCH-CON-01 - Client PII UAE Resident Only**

## Decision

ADR-007 - Milvus over Qdrant for vector storage

## Rationale

We chose Milvus because: (1) Ceph S3 backend - integrates with our Ceph RGW S3 storage (UAE-resident); (2) Sovereign data residency - we control all infrastructure, no vendor lock-in; (3) Scales horizontally - Milvus partitions data across nodes (future-proof for millions of profiles).

Alternative: Qdrant. Rejected because: weaker Ceph integration, single-node limitations for profile scale.

## Consequences

✅ **Positive:** Client embeddings stay in UAE, scales to millions of profiles, fully controlled infrastructure, integration with Ceph.

⚠️ **Negative:** Operational overhead (Milvus deployment), fewer managed service options.
   - Mitigation: Use Milvus Operator for Kubernetes-style management (Phase 2).

## Implementation Evidence

### Related Solution Architectures

- **SAD-006 - LangGraph state machine and SGLang inference topology** - See SAD for implementation details

## Compliance & Governance

### Standards & Frameworks
- TOGAF Architecture Framework (ADR best practices)
- UAE PDPL (Personal Data Protection Law)
- ISO 27001 (Information Security Management)
- CBUAE Guidelines (Central Bank AI Governance)

---

**Document Owner:** Cedric (Lead AI Enterprise Architect)
**Last Updated:** 2026-04-05
**Review Cycle:** Quarterly (minimum)
