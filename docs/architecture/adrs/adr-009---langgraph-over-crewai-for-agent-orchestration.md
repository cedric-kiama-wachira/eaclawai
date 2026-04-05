# ADR-009 - LangGraph over CrewAI for agent orchestration

**Date:** 2026-04-05
**Status:** Accepted
**Decider:** Roberto (Head of AI), Cedric (Lead AI Enterprise Architect)
**Stakeholders:** Platform, Security, Compliance

---

## Principles Driving This Decision

- **P6 - Observability by Design**

## Context

Agents are the foundation of Elite Gold and future products. LangGraph and CrewAI are both multi-agent frameworks. LangGraph is a state machine (explicit, auditable). CrewAI is a task-based framework (implicit, harder to debug).

ARCH-CON-06 requires every recommendation to be explainable. State machines are inherently explainable - you can see exactly what state the agent was in, what decision it made, why.

### Applicable Constraints

- **ARCH-CON-06 - Every AI recommendation must be explainable and auditable**
- **ARCH-CON-09 - All agent outputs evaluated before RM delivery**

## Decision

ADR-009 - LangGraph over CrewAI for agent orchestration

## Rationale

We chose LangGraph because: (1) State machine transparency - every agent step is auditable (critical for CBUAE compliance); (2) Lowest latency - 3x faster than CrewAI for multi-agent workflows (important for interactive RM tools); (3) Aligns with P1 (Agentic by Design) - LangGraph's state graphs are the foundation of agentic architecture.

Alternative: CrewAI. Rejected because: slower (iterative refinement pattern), opaque state transitions, harder to explain to regulators.

## Consequences

✅ **Positive:** Auditable agents, low latency, transparent state transitions, foundation for 15-20 products, industry-leading framework.

⚠️ **Negative:** Smaller ecosystem than CrewAI, fewer pre-built agent templates.
   - Mitigation: Build templates for Elite Gold use case (RM, product, signal agents).

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
