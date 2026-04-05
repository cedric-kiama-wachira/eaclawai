# SAD-004 - Explainability and client feedback loop design

**Date:** 2026-04-05
**Status:** Approved
**Author:** Cedric (Lead AI Enterprise Architect)

---

## Executive Summary

This document describes how every AI recommendation is explained to clients and how feedback loops enable continuous improvement.

The feedback loop is the mechanism for: (1) RM approval gate, (2) client explainability, (3) continuous model improvement.

## Related Architecture Decisions

- **ADR-004 - Client feedback loop embedded in every AI recommendation cycle**

## Architecture

**Explainability Pipeline:**
1. Agent generates recommendation with sources (e.g., "Based on your portfolio holdings in tech, your 5-year risk profile, and current market valuations...")
2. LLM Guard validates recommendation is explainable (source citations present, no vague claims)
3. Langfuse logs the "why" (retrieved context, model inputs, reasoning trace)
4. RM sees explanation in Chainlit UI, approves/rejects
5. Client sees simplified explanation in mobile app (if recommendation is approved)
6. Client provides feedback (rating, additional context)

**Feedback Loop:**
- Client feedback stored in PostgreSQL (audit trail)
- Feedback updates Mem0 client profile (if client provided new preferences)
- Feedback aggregated for model retraining signal (which recommendation types clients accept)
- Monthly feedback analysis informs model improvements

## Technology Stack

- **LLM Guard - Prompt injection, data leakage prevention**
- **Langfuse - LLM observability, token cost, quality traces**
- **PostgreSQL 16 - Relational, audit log, consent records**

- **LLM Guard:** Validates output is explainable (no hallucinations, sources cited)
- **Langfuse:** Logs full recommendation trace (context, inputs, reasoning, tokens)
- **PostgreSQL:** Stores RM approvals and client feedback
- **Mem0:** Updates client profile based on feedback
- **Chainlit:** RM approval UI with explanation display
- **Mobile App:** Client-facing UI for recommendations and feedback

## Success Criteria

- [ ] 100% of recommendations have explanation logged in Langfuse
- [ ] 0 recommendations sent to client without RM approval (gate working)
- [ ] 80%+ of recommendations have source citations (explainability)
- [ ] Client feedback collection >50% (engagement)
- [ ] Monthly feedback analysis identifies 2-3 model improvements
- [ ] RM approval rate stays >85% (agent reliability)

## Deployment

**Phase 1:** Explainability UI in Chainlit for RMs, no client-facing feedback yet.
**Phase 2:** Integrate with FAB mobile app, enable client feedback collection.

---

**Document Owner:** Cedric (Lead AI Enterprise Architect)
**Last Updated:** 2026-04-05
**Review Cycle:** Quarterly (minimum)
