# ADR-010 - Mem0 over Letta for agent memory

**Date:** 2026-04-05
**Status:** Accepted
**Decider:** Roberto (Head of AI), Cedric (Lead AI Enterprise Architect)
**Stakeholders:** Platform, Security, Compliance

---

## Principles Driving This Decision

- **P1 - Agentic by Design**

## Context

RMs interact with agents repeatedly: "Show me a client brief... now add portfolio data... now what about risk?". The agent must remember context across turns (client identity, conversation history, RM preferences). This is agent memory.

Mem0 and Letta both provide persistent memory APIs. Mem0 is faster (vector-based retrieval), Letta is more flexible (custom memory structure). For our use case (RM iterative workflows), speed matters more.

### Applicable Constraints

- **ARCH-CON-01 - Client PII UAE Resident Only**

## Decision

ADR-010 - Mem0 over Letta for agent memory

## Rationale

We chose Mem0 because: (1) Speed - vector retrieval is 10-50ms, enabling real-time RM interactions; (2) Simplicity - API is elegant and easy to integrate into LangGraph; (3) Aligns with P1 (Agentic by Design) - memory is essential for autonomous agents; (4) Client context persistence - RM can context-switch between clients without re-briefing the agent.

Alternative: Letta. Rejected because: slower (token-based retrieval), more operational complexity.

## Consequences

✅ **Positive:** Fast context retrieval, persistent client memory, enables agent autonomy, improves RM productivity.

⚠️ **Negative:** Additional infrastructure component, requires tuning for optimal retrieval quality.
   - Mitigation: Start with simple vector embeddings, add semantic ranking in Phase 2.

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
