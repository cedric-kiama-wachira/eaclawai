# SAD-002 - RM co-pilot and lounge recognition design

**Date:** 2026-04-05
**Status:** Approved
**Author:** Cedric (Lead AI Enterprise Architect)

---

## Executive Summary

This document describes the two agents that transform Elite Gold relationship management:

**RM Co-Pilot (elitegold-rm):** Generates personalized client briefs, recommended actions, and portfolio insights for RMs during morning prep and client meetings.

**Lounge Recognition (elitegold-lounge):** Recognizes Elite Gold clients arriving at FAB lounge, briefs RM on arrival, enables "personalized greeting" experience.

## Related Architecture Decisions

- **ADR-002 - Elite Gold as Phase 1 before HNWI**

## Agent Orchestration

**RM Co-Pilot Agent State Machine:**
- State 1: RETRIEVE - Query Mem0 + Milvus for client context (conversation history, portfolio, behavior patterns)
- State 2: GATHER - Call MCP Server for real-time data (account status, recent transactions, compliance flags)
- State 3: GENERATE - SGLang generates brief, actionable insights, and recommended next steps
- State 4: VALIDATE - LLM Guard checks output for: hallucinations, policy violations, appropriate language
- State 5: PRESENT - Return brief to RM via Chainlit UI, wait for RM approval
- State 6: FEEDBACK - Log approval/rejection, update Mem0 if feedback available

**Lounge Recognition Agent State Machine:**
- State 1: RECOGNIZE - Face/ID scan triggers agent, queries client database via MCP Server
- State 2: BRIEF - Query Mem0 + Milvus for client profile (preferred communication style, known preferences)
- State 3: GENERATE - Create 30-second greeting script with personalized details
- State 4: ALERT - Alert RM that client has arrived with personalized greeting context
- State 5: FEEDBACK - Log whether greeting was well-received (RM accepts/modifies script)

## Technology Stack

- **elitegold-rm - RM Co-Pilot Agent**
- **elitegold-lounge - Phygital Lounge Agent**
- **RM Daily Brief Service**
- **Lounge Recognition Service**
- **LLM Guard - Prompt injection, data leakage prevention**
- **Langfuse - LLM observability, token cost, quality traces**

- **LangGraph:** State machine orchestration (auditable, explicit state transitions)
- **SGLang:** Brief generation and greeting script generation (fast CPU inference)
- **Mem0:** Client conversation memory (retrieval of past interactions and preferences)
- **Milvus:** Client profile embeddings (holdings, risk, communication style)
- **LLM Guard:** Output validation (no hallucinations, policy compliance)
- **Langfuse:** Tracing every brief and greeting (cost attribution, quality feedback)
- **Chainlit:** RM-facing UI for brief approval and feedback

## Success Criteria

- [ ] RM brief generation <2 seconds (real-time iterations possible)
- [ ] 80%+ of RMs report brief accuracy (client context complete)
- [ ] LLM Guard catches 100% of policy violations (zero bad recommendations to clients)
- [ ] RM approval rate >85% (agent is trustworthy)
- [ ] Client satisfaction +10 NPS points (personalized experience)
- [ ] RM time savings >5 hours/week (admin to relationships)

## Deployment

**Phase 1:** Local testing with 2-3 Elite Gold RMs, iterate on brief quality, refine Mem0 embeddings.
**Phase 2:** Scale to 10 RMs, add lounge recognition (requires biometric integration), evaluate Phase 3 expansion to other RM tiers.

---

**Document Owner:** Cedric (Lead AI Enterprise Architect)
**Last Updated:** 2026-04-05
**Review Cycle:** Quarterly (minimum)
