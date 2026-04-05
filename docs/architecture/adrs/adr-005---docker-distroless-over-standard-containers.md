# ADR-005 - Docker Distroless over standard containers

**Date:** 2026-04-05
**Status:** Accepted
**Decider:** Roberto (Head of AI), Cedric (Lead AI Enterprise Architect)
**Stakeholders:** Platform, Security, Compliance

---

## Principles Driving This Decision

- **P5 - Zero Trust Security**

## Context

ARCH-CON-08 requires all containers run non-root with no shell. Standard container images (Ubuntu, Alpine) include ~200MB of OS utilities, shells, and package managers - all unnecessary for running compiled binaries. These increase attack surface and slow deployments.

Distroless images contain only the application and its runtime - no shell, no package manager, no OS utilities. This is the hardest container to exploit.

### Applicable Constraints

- **ARCH-CON-08 - All containers run non-root with no shell**

## Decision

ADR-005 - Docker Distroless over standard containers

## Rationale

We chose distroless because: (1) Meets ARCH-CON-08 requirement (non-root, no shell); (2) Security hardening - removes 90% of potential attack vectors; (3) Smaller images (10-50MB vs 200MB) - faster deployments; (4) Aligns with P5 (Zero Trust) - assume every image will be scanned and attacked.

Alternative: Alpine Linux. Rejected because: still includes shell, package manager, and OS tools.

## Consequences

✅ **Positive:** Hardened containers, faster deployments, smaller images, security compliance, passes all security scans.

⚠️ **Negative:** Harder to debug (no shell), can't install tools inside container.
   - Mitigation: Debug sidecars for troubleshooting, all debugging done in test environment.

## Implementation Evidence

### Related Solution Architectures

- **SAD-005 - Distroless multi-stage Docker build pattern** - See SAD for implementation details

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
