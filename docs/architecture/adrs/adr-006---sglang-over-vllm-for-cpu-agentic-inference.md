# ADR-006 - SGLang over vLLM for CPU agentic inference

**Date:** 2026-04-05
**Status:** Accepted
**Decider:** Roberto (Head of AI), Cedric (Lead AI Enterprise Architect)
**Stakeholders:** Platform, Security, Compliance

---

## Principles Driving This Decision

- **P7 - Portability First - Phase 1 local equals Phase 2 Azure**

## Context

Phase 1 deployment on Hetzner has 64GB RAM and CPU-only (no GPU). We need to run Qwen2.5-7B quantized model (Q4, ~4GB) efficiently. Both SGLang and vLLM support CPU inference, but have different performance characteristics for the CPU-bound case.

SGLang uses RadixAttention kernel for KV cache reuse - important for multi-turn conversations where the RM is iterating on client briefs. vLLM has broader GPU support but less optimized CPU kernels.

### Applicable Constraints

- **ARCH-CON-08 - All containers run non-root with no shell**

## Decision

ADR-006 - SGLang over vLLM for CPU agentic inference

## Rationale

We chose SGLang because: (1) RadixAttention optimization - 30-50% faster for CPU inference on multi-turn (perfect for RM iterative workflow); (2) Lower memory footprint - critical on 64GB shared system; (3) Aligns with P7 (Portability) - same inference engine Phase 1 (Hetzner CPU) and Phase 2 (Azure GPU).

Alternative: vLLM. Rejected because: slower on CPU, doesn't justify learning two inference engines.

## Consequences

✅ **Positive:** Fast CPU inference, low memory, multi-turn optimization, same engine across phases, meets performance targets (<100ms per token).

⚠️ **Negative:** Smaller ecosystem than vLLM, fewer third-party integrations.
   - Mitigation: Route through LiteLLM for abstraction.

## Implementation Evidence

### Related Solution Architectures

- **SAD-006 - LangGraph state machine and SGLang inference topology** - See SAD for implementation details

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
