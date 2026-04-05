# SAD-007 - Vault dynamic credentials and secret injection pattern

**Date:** 2026-04-05
**Status:** Approved
**Author:** Cedric (Lead AI Enterprise Architect)

---

## Executive Summary

This document describes how all secrets (database passwords, API keys, LLM credentials) are managed using HashiCorp Vault with dynamic credential generation and automatic rotation.

This implements ARCH-CON-07: "No secrets in environment variables or config files."

## Related Architecture Decisions

- **ADR-011 - HashiCorp Vault for secrets management**

## Architecture

**Vault Setup:**
- Raft storage with 3 nodes (HA consensus)
- PKI secrets engine for TLS certificates (auto-renew 30 days before expiry)
- Database secrets engine for Postgres credentials (rotate every 30 days)
- API endpoint at vault.eaclaw.ai (internal only)

**Credential Injection:**
1. Container starts, requests credentials from Vault (service account JWT)
2. Vault validates JWT (signed by Vault CA), checks RBAC policy
3. Vault returns: database password (valid for 30 days), API keys (rotated hourly)
4. Container uses credentials, Vault tracks all accesses
5. On credential expiry, container requests new credentials (seamless rotation)

## Technology Stack

- **HashiCorp Vault - Secrets management, PKI, dynamic credentials**

- **HashiCorp Vault:** Secret management, PKI, dynamic credentials
- **Raft:** Consensus-based HA (no external database needed)
- **Docker Compose / Kubernetes:** Inject Vault tokens at container start
- **PostgreSQL:** Native secret rotation (Vault creates temporary roles)

## Audit

**Audit Trail:**
- Every secret request logged to Vault audit log
- Every credential rotation logged
- Every failed auth attempt logged
- Monthly audit: "Who requested which secret when and from where"
- Compliance: Exportable audit log for regulatory review

## Success Criteria

- [ ] Zero secrets in Docker compose files or code (all from Vault)
- [ ] All credentials rotated automatically (no manual intervention)
- [ ] 100% of secret accesses logged (audit trail complete)
- [ ] Vault availability >99.9% (critical component)
- [ ] Credential rotation never causes service disruption
- [ ] Monthly audit shows all secrets rotated successfully

## Deployment

**Phase 1:** Local Vault with Raft storage (3-node consensus on single machine).
**Phase 2:** Vault moved to 3 dedicated Azure nodes with HA failover.

---

**Document Owner:** Cedric (Lead AI Enterprise Architect)
**Last Updated:** 2026-04-05
**Review Cycle:** Quarterly (minimum)
