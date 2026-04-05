# SAD-001 - Sovereign + Azure UAE North topology

**Date:** 2026-04-05
**Status:** Approved
**Author:** Cedric (Lead AI Enterprise Architect)

---

## Executive Summary

This document describes the two-phase infrastructure topology for AARM-EG:

**Phase 1 (Local Sovereign):** Single Hetzner dedicated server (64GB RAM, Intel i5-13500, no GPU) running 21 containers on Docker bridge network. All data stays on premise, validated locally before any cloud movement.

**Phase 2 (Azure UAE North):** Migrate to Azure UAE North with CAF landing zones, private endpoints, and HA failover. Same application stack, Azure-managed infrastructure.

## Related Architecture Decisions

- **ADR-001 - Sovereign stack before Azure for PII**

## Architecture

**Phase 1 Topology:**
- Hetzner node (192.168.x.x) runs 4 Docker networks: api-lab-net (172.20.0.0/24), df-platform-net, df-data-net, df-obs-net
- SGLang inference (1 container), LangGraph orchestration, LiteLLM routing
- Milvus + Ceph RGW S3 (vector embeddings and file storage)
- PostgreSQL 16 (audit log, consent records)
- Keycloak 26 (IAM - UAE identity), Vault (secrets with PKI CA)
- Langfuse (observability), Prometheus + Grafana (metrics)
- APISIX gateway (API management, rate limiting)
- Traefik v3 (TLS termination, wildcard cert for eaclaw.ai)

**Phase 2 Topology:**
- Azure UAE North landing zones (production, staging, dev)
- AKS cluster with 3 namespaces mirroring Docker networks
- Same container images, same secrets from Vault
- Managed PostgreSQL, Managed Keycloak (Azure AD integration)
- Private endpoints for all resources
- Azure Traffic Manager for RM failover (global RM assignment)

## Data Flow

**Client Brief Generation Flow:**
1. RM triggers brief request via Chainlit UI → APISIX (auth token from Keycloak)
2. APISIX routes to elitegold-rm agent (LangGraph state machine)
3. Agent queries Mem0 (client conversation history) + Milvus (client profile vector)
4. Agent calls MCP Server for CRM context (last interactions, holdings, risk profile)
5. Agent generates brief with SGLang, routes through LiteLLM for cost tracking
6. LLM Guard validates recommendation (no hallucinations, compliant language)
7. Langfuse logs every step (retrieval, generation, tokens, latency)
8. RM sees brief in Chainlit, approves/rejects
9. Approval stored in PostgreSQL audit log
10. Feedback updates Mem0 client profile vector

## Technology Stack

- **Keycloak 26 - IAM, RBAC, OIDC, three clients**
- **HashiCorp Vault - Secrets management, PKI, dynamic credentials**
- **TLS 1.3 - eaclaw.ai wildcard certificate**
- **Docker Engine - Distroless containers**

## Security & Compliance

**Data Residency:** All PII (client embeddings, interaction history, recommendations) stored in Ceph RGW S3 on premise (Phase 1) or Azure UAE North (Phase 2). No third-party systems have access.

**Access Control:** Keycloak RBAC (role-based), MFA for all human users, service accounts use Vault dynamic credentials (auto-rotate). Every API call authenticated, every database access audited.

**Audit Trail:** PostgreSQL audit log (Postgres native), Langfuse trace log (every LLM call), Vault secret access log (every credential rotation), Keycloak audit (every user action).

## Success Criteria

- [ ] PII stays in UAE (audit log shows zero external data movement)
- [ ] All recommendations logged in Langfuse (100% traceability)
- [ ] LLM Guard validates all outputs before RM delivery (zero policy violations)
- [ ] Response latency <2 seconds (RM can iterate in real time)
- [ ] System availability >99.5% (RM depends on platform)
- [ ] All secrets rotate automatically (no manual rotation)
- [ ] Zero security findings from internal/external audit

## Deployment

**Phase 1 Deployment:** Hetzner server pre-provisioned, Docker Compose stack defined in eaclawAI/docs/docker-compose.yml, validated locally, demonstrated to FAB stakeholders.

**Phase 2 Deployment:** Azure IaC (Terraform) for landing zones, AKS cluster, managed services. Cutover plan: shadow traffic 1 week, gradual traffic shift 50/50, full cutover, keep Hetzner as disaster recovery.

**Rollback:** If Azure migration fails, revert to Phase 1 + Hetzner disaster recovery node. Keep Phase 1 infrastructure alive for 30 days post-cutover.

---

**Document Owner:** Cedric (Lead AI Enterprise Architect)
**Last Updated:** 2026-04-05
**Review Cycle:** Quarterly (minimum)
