# ADR-001 - Sovereign stack before Azure for PII

**Date:** 2026-04-05
**Status:** Accepted
**Decider:** Roberto (Head of AI), Cedric (Lead AI Enterprise Architect)
**Stakeholders:** Platform, Security, Compliance

---

## Principles Driving This Decision

- **P4 - Sovereignty First - UAE PDPL**

## Context

FAB Elite Gold program requires client PII to remain within UAE jurisdiction to comply with UAE Personal Data Protection Law (PDPL). Azure cloud services were initially considered, but regulatory analysis determined that customer trust and data sovereignty demands a phased approach: prove the platform works locally first (Phase 1 on Hetzner), then migrate to Azure UAE North (Phase 2) only after validation.

Client data includes account history, transaction records, and behavioral profiles used for AI recommendations. These are classified as PII under UAE PDPL and must never leave UAE territory.

### Applicable Constraints

- **ARCH-CON-01 - Client PII UAE Resident Only**
- **ARCH-CON-05 - Azure UAE North Target Platform**

## Decision

ADR-001 - Sovereign stack before Azure for PII

## Rationale

We chose to deploy sovereign infrastructure first because: (1) De-risks regulatory compliance - we control all data movement; (2) Builds stakeholder confidence - RM teams and clients see their data stays local; (3) Enables iterative validation - we test the model, data flows, and compliance controls before cloud migration; (4) Aligns with P4 (Sovereignty First) - demonstrates commitment to data protection.

Alternative: Deploy directly to Azure UAE North. Rejected because: lacks validation on local infrastructure, carries regulatory risk if cloud migration hits unforeseen issues, doesn't demonstrate local data residency capability.

## Consequences

✅ **Positive:** Demonstrates data sovereignty capability, reduces regulatory risk, builds confidence for Phase 2 migration, enables testing of compliance controls, meets PDPL requirements.

⚠️ **Negative:** Single point of failure in Phase 1 (Hetzner), higher operational overhead, requires cross-data-center failover design for Phase 2.
   - Mitigation: HA design in SAD-001, plan failover before Phase 2 cutover.

## Implementation Evidence

### Related Solution Architectures

- **SAD-001 - Sovereign + Azure UAE North topology** - See SAD for implementation details

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
