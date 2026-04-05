# ADR-008 - Langfuse over LangSmith for LLM observability

**Date:** 2026-04-05
**Status:** Accepted
**Decider:** Roberto (Head of AI), Cedric (Lead AI Enterprise Architect)
**Stakeholders:** Platform, Security, Compliance

---

## Principles Driving This Decision

- **P6 - Observability by Design**

## Context

Every AI recommendation involves multiple model calls (retrieval, ranking, generation). We need to log every token (for cost attribution), every latency (for SLA tracking), and every quality signal (was the recommendation accepted?).

LangSmith is Langchain's commercial offering. Langfuse is open-source, self-hosted alternative. Both log token usage and latency, but Langfuse allows self-hosting (UAE data residency) and lower cost.

### Applicable Constraints

- **ARCH-CON-06 - Every AI recommendation must be explainable and auditable**

## Decision

ADR-008 - Langfuse over LangSmith for LLM observability

## Rationale

We chose Langfuse because: (1) Self-hosted - client recommendation data stays in UAE, no vendor access; (2) Cost - self-hosted is 1/10th the price of LangSmith; (3) Aligns with P6 (Observability by Design) - every recommendation is traceable; (4) ARCH-CON-06 enforcement (explainability) - Langfuse traces show exactly what the model saw and why.

Alternative: LangSmith. Rejected because: vendor-hosted (data leaves UAE), higher cost, less customizable.

## Consequences

✅ **Positive:** Full token tracing, cost attribution, recommendation traceability, compliance with explainability requirement, improved model quality through feedback loops.

⚠️ **Negative:** Operational overhead (Langfuse deployment), no vendor SLA.
   - Mitigation: Redundant Langfuse nodes in Phase 2.

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
