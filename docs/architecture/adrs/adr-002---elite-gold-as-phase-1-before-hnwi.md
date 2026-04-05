# ADR-002 - Elite Gold as Phase 1 before HNWI

**Date:** 2026-04-05
**Status:** Accepted
**Decider:** Roberto (Head of AI), Cedric (Lead AI Enterprise Architect)
**Stakeholders:** Platform, Security, Compliance

---

## Principles Driving This Decision

- **P3 - Human Authority Over AI**

## Context

FAB's AI Innovation Hub targets 15-20 AI products across customer segments. Elite Gold (high-net-worth relationship managers) is the most resource-constrained use case: 50-100 RMs managing 10-15 clients each, spending 70-75% of time on admin instead of relationships. This is where AI can show immediate ROI.

HNWI (ultra-high-net-worth) segment is higher risk - fewer clients per RM, complex financial structures, regulatory scrutiny. Validating Elite Gold first de-risks the roadmap.

### Applicable Constraints

- **ARCH-CON-02 - RM Approval Before Client Contact**
- **ARCH-CON-06 - Every AI recommendation must be explainable and auditable**

## Decision

ADR-002 - Elite Gold as Phase 1 before HNWI

## Rationale

We prioritize Elite Gold because: (1) Highest ROI - every RM can work with more clients immediately; (2) Validates agent architecture - agents must work in a real business process (RM daily workflow); (3) De-risks roadmap - proves AI governance works before HNWI complexity.

Alternative: Target HNWI segment first. Rejected because: higher complexity (custom structures, regulatory documentation), fewer clients means slower feedback loop, success metrics harder to measure.

## Consequences

✅ **Positive:** Quick wins demonstrate AI value, agents proven in real workflow, governance framework validated, foundation for HNWI and other segments.

⚠️ **Negative:** Elite Gold RMs may resist AI (change management), requires LLM Guard validation of all recommendations.
   - Mitigation: Training program, gradual rollout, explainability dashboard.

## Implementation Evidence

### Related Solution Architectures

- **SAD-002 - RM co-pilot and lounge recognition design** - See SAD for implementation details

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
