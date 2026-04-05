# SAD-006 - LangGraph state machine and SGLang inference topology

**Date:** 2026-04-05
**Status:** Approved
**Author:** Cedric (Lead AI Enterprise Architect)

---

## Executive Summary

This document describes the core agent orchestration architecture: LangGraph state machines controlling SGLang inference, with memory (Mem0), context (Milvus), and observability (Langfuse).

This is the foundation of all 5 Elite Gold agents (RM, product, signal, lounge, FinOps).

## Related Architecture Decisions

- **ADR-009 - LangGraph over CrewAI for agent orchestration**
- **ADR-006 - SGLang over vLLM for CPU agentic inference**
- **ADR-007 - Milvus over Qdrant for vector storage**
- **ADR-008 - Langfuse over LangSmith for LLM observability**
- **ADR-010 - Mem0 over Letta for agent memory**

## State Machines

**Common Agent State Graph:**
State 1: RETRIEVE → Query Mem0 + Milvus (conversation history + profile)
State 2: GATHER → Call MCP Server (real-time CRM/pricing data)
State 3: GENERATE → SGLang inference (brief, recommendations, insights)
State 4: VALIDATE → LLM Guard (check policy compliance)
State 5: PRESENT → Return to RM/client via UI
State 6: FEEDBACK → Log outcome for continuous improvement

**Routing:**
- Input → Dispatcher agent (routes to RM/product/signal/lounge agent)
- Each agent is a separate state machine, can run in parallel
- LiteLLM router handles model selection (SGLang CPU, future GPU alternatives)

## Inference Configuration

**SGLang Configuration:**
- Model: Qwen2.5-7B-Q4 (quantized to 4-bit, ~4GB memory)
- Context window: 8K tokens (enough for client history + request)
- Temperature: 0.7 (creative but not hallucinating)
- Top-p: 0.95 (diverse but coherent)
- Max tokens: 500 (brief generation)

**Performance Targets:**
- Latency: <100ms per token (real-time RM interactions)
- Throughput: 1 brief per 2 seconds (scale to multiple RMs)
- Memory: <16GB (leave headroom on 64GB system)

## Technology Stack

- **LangGraph - Agent state machine, auditable, lowest latency**
- **SGLang - CPU inference, RadixAttention, Qwen2.5-7B-Q4**
- **LiteLLM - Model router, OpenAI-compatible, cost tracking**
- **Milvus - Vector store, Ceph S3 backend, client profiles**
- **Mem0 - Agent memory, client context persistence**
- **Langfuse - LLM observability, token cost, quality traces**

- **LangGraph:** State machine orchestration, explicit decision points
- **SGLang:** CPU inference with RadixAttention optimization
- **LiteLLM:** Model routing (cost tracking, fallback models)
- **Mem0:** Client conversation memory (vector storage)
- **Milvus:** Client profile embeddings (portfolio, behavior, risk)
- **MCP Server:** Real-time data access (CRM, pricing, compliance)
- **LLM Guard:** Output validation (policy compliance)
- **Langfuse:** Full tracing (latency, tokens, quality)
- **Prometheus:** Infrastructure metrics (CPU, memory, inference latency)

## Success Criteria

- [ ] Response latency <2 seconds (RM can iterate in real time)
- [ ] CPU usage <80% with 10 concurrent requests (room to scale)
- [ ] 100% of inferences traced in Langfuse (no blind spots)
- [ ] Zero LLM Guard violations (all outputs valid)
- [ ] Token cost <$0.01 per brief (economically viable)
- [ ] Mem0 retrieval accuracy >85% (context is relevant)

## Deployment

**Phase 1:** Single SGLang instance on CPU, 2-3 RMs testing agents, iterate on quality.
**Phase 2:** Scale to 20+ RMs, add GPU backup for throughput, implement load balancing.

---

**Document Owner:** Cedric (Lead AI Enterprise Architect)
**Last Updated:** 2026-04-05
**Review Cycle:** Quarterly (minimum)
