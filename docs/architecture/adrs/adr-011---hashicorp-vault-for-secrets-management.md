# ADR-011 - HashiCorp Vault for secrets management

**Date:** 2026-04-05
**Status:** Accepted
**Decider:** Roberto (Head of AI), Cedric (Lead AI Enterprise Architect)
**Stakeholders:** Platform, Security, Compliance

---

## Principles Driving This Decision

- **P7 - Portability First - Phase 1 local equals Phase 2 Azure**

## Context

ARCH-CON-07 prohibits secrets in environment variables or config files. Secrets include: database passwords, API keys (CRM, pricing, consent), LLM model keys, Keycloak admin credentials. These must be rotated regularly, audited, and never exposed.

HashiCorp Vault is the enterprise standard for secret management. It supports dynamic secrets (auto-rotate), audit logging (every access), and encryption at rest.

### Applicable Constraints

- **ARCH-CON-07 - No secrets in environment variables or config files**

## Decision

ADR-011 - HashiCorp Vault for secrets management

## Rationale

We chose Vault because: (1) Meets ARCH-CON-07 requirement (secrets never in config); (2) Dynamic credentials - database passwords auto-rotate, reducing compromise window; (3) Audit trail - every secret access is logged (compliance requirement); (4) Aligns with P5 (Zero Trust) - assume every system will be compromised, so rotate secrets constantly.

Alternative: Environment-variable-based secrets. Rejected because: violates ARCH-CON-07, no audit trail, no auto-rotation.

## Consequences

✅ **Positive:** Secrets never leaked in logs, auto-rotation reduces breach impact, full audit trail, complies with security standards.

⚠️ **Negative:** Additional infrastructure (Vault HA), operational overhead (PKI management).
   - Mitigation: Vault Raft storage for HA, Kubernetes integration in Phase 2.

## Implementation Evidence

### Related Solution Architectures

- **SAD-007 - Vault dynamic credentials and secret injection pattern** - See SAD for implementation details

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
