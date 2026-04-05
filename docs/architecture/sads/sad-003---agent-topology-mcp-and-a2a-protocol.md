# SAD-003 - Agent topology MCP and A2A protocol

**Date:** 2026-04-05
**Status:** Approved
**Author:** Cedric (Lead AI Enterprise Architect)

---

## Executive Summary

This document describes how agents communicate with each other and access enterprise tools.

**MCP Server:** Golang microservice exposing FAB systems (CRM, pricing, product catalogue, compliance API) as tools that agents can call.

**A2A Protocol:** Agent-to-agent communication using LangGraph state transitions. Agents coordinate workflows (e.g., RM agent calls product agent for recommendations).

## Related Architecture Decisions

- **ADR-003 - MCP and A2A as agent protocol**

## Architecture

**MCP Server (Go):**
- REST API endpoints for: /customers/{id}, /products/{category}, /pricing/{product}, /compliance/{client_id}
- Authentication: Service account with Vault-issued JWT (rotated hourly)
- Logging: Every API call logged to Langfuse and PostgreSQL audit
- Versioning: Semantic versioning, backwards-compatible API design

**Agent Communication:**
- RM agent generates brief → calls Product agent for recommendations
- Product agent queries /products/{category} via MCP Server → returns top 5 products
- RM agent includes product recommendations in brief
- All calls traced end-to-end in Langfuse

## Technology Stack

- **MCP Server (Golang) - Tool access, CRM, product catalogue**

- **MCP Server (Golang):** Exposes enterprise APIs as agent tools
- **LangGraph:** Defines state machine for agent-to-agent communication
- **Vault:** Service account credentials for MCP Server (rotated hourly)
- **Langfuse:** Traces every MCP call (who called, what data, when, latency)

## Success Criteria

- [ ] MCP Server availability >99.9% (agents depend on it)
- [ ] API latency <500ms (agent interactions must be fast)
- [ ] 100% of MCP calls traced in Langfuse (audit compliance)
- [ ] Zero credential leaks (Vault rotation working)
- [ ] New agent tools added in <1 day (extensibility)

## Deployment

**Phase 1:** MCP Server running locally with mocked CRM/pricing APIs for testing.
**Phase 2:** Connect to real FAB systems (CRM, pricing, compliance) with staged rollout.

---

**Document Owner:** Cedric (Lead AI Enterprise Architect)
**Last Updated:** 2026-04-05
**Review Cycle:** Quarterly (minimum)
