# ADR-003 - MCP and A2A as agent protocol

**Date:** 2026-04-05
**Status:** Accepted
**Decider:** Roberto (Head of AI), Cedric (Lead AI Enterprise Architect)
**Stakeholders:** Platform, Security, Compliance

---

## Principles Driving This Decision

- **P1 - Agentic by Design**

## Context

FAB's 15-20 AI products will have multiple agents (RM agents, product agents, signal agents, etc.). These agents must call tools (CRM, pricing, product catalogue) and communicate with each other (agent-to-agent workflows). We need a standard protocol that is auditable (every tool call traced), standardized (consistent across all agents), and extensible (easy to add new tools).

Model Context Protocol (MCP) is the emerging standard for AI agent tool integration. Agent-to-Agent (A2A) is the communication pattern for multi-agent orchestration.

### Applicable Constraints

- **ARCH-CON-06 - Every AI recommendation must be explainable and auditable**

## Decision

ADR-003 - MCP and A2A as agent protocol

## Rationale

We chose MCP + A2A because: (1) Industry standard - MCP becoming the de facto standard for LLM tool access; (2) Full auditability - every tool call is logged, traceable, compliant with ARCH-CON-06 (explainable); (3) Extensibility - adding new tools/agents doesn't require code changes to orchestration layer.

Alternative: Custom protocol. Rejected because: reinvents the wheel, no standards community, harder to audit, blocks partnerships.

## Consequences

✅ **Positive:** Agents are auditable and extensible, industry-aligned tooling, faster agent development, CBUAE compliance (transaction tracing).

⚠️ **Negative:** MCP ecosystem still maturing, fewer third-party tools available today.
   - Mitigation: Build MCP Server (Golang) as central hub for FAB-specific tools.

## Implementation Evidence

### Related Solution Architectures

- **SAD-003 - Agent topology MCP and A2A protocol** - See SAD for implementation details

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
