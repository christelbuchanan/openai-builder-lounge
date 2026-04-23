# OpenAI Builder Lounge Repo — Content Summary

This repository contains a small set of architecture notes describing the **ChatAndBuild agent runtime**: how a chat “thread” orchestrates model calls via the **OpenAI Responses API**, how tools are executed server-side under policy, how memory/RAG is scoped, and how streaming/observability works.

> Note: GitHub will render **`README.md`** by default. This repo also contains `readme.md` (same content) for backward compatibility.

## Files at a glance

### 1) `README.md` / `readme.md` — *Message Turn Sequence (runtime flow)*
**What it is:** A time-ordered, end-to-end walkthrough of a **single message turn** from UI → API → thread loading → memory retrieval → model routing → Responses API → tool policy → tool runner → streaming → final payload.

- Open: [README.md](./README.md)

**Highlights:**
- Mermaid **sequence diagram** showing the full loop (including tool approval/denial branches)
- Clear phases:
  - Request & context loading
  - Routing & model selection
  - Streaming token delivery
  - Tool execution loop with policy gate
  - Final response + structured JSON blocks
- Emphasis on **observable UX** (token deltas + tool lifecycle events)
- Performance targets (TTFT, retrieval, tool validation, end-to-end)

**Best for:** Explaining “what happens when a user sends a message?”

---

### 2) `system-architecture.md` — *Agent runtime architecture (components)*
**What it is:** A component-level view of the runtime: Thread, Router, Responses API, Tool Runner, Memory/RAG, Event Stream, and an optional Realtime Worker (voice).

- Open: [system-architecture.md](./system-architecture.md)

**Highlights:**
- Mermaid **flowchart** showing the core agent loop + tool loop + memory injection + streaming
- Defines responsibilities for:
  - **Thread** as state container (agent spec, policy, scope, receipts)
  - **Router** for model selection (task/risk/latency/budget)
  - **Tool Runner** for server-side execution + sanitization + receipts
  - **Memory/RAG** using `text-embedding-3-small` + MongoDB vector index
  - **Event Stream** for token/tool/JSON streaming + metrics
  - **Voice** path via LiveKit + RealtimeModel

**Best for:** Onboarding someone to the system’s major building blocks.

---

### 3) `architecture-by-planes.md` — *Four-plane enterprise systems view*
**What it is:** A higher-level infrastructure view organized into four planes:
- **Control Plane** (API + orchestration)
- **Data Plane** (state + retrieval)
- **Event Plane** (streaming + jobs + observability)
- **Session Plane** (realtime voice/avatar + MCP tools)

- Open: [architecture-by-planes.md](./architecture-by-planes.md)

**Highlights:**
- Mermaid **systems diagram** grouped by planes
- Concrete tech choices called out (NestJS, MongoDB + vector search, Redis + BullMQ, Socket.IO, LiveKit)
- Detailed sections on:
  - scoped retrieval and indexing
  - async tool execution patterns
  - observability metrics (TTFT, schema pass rate, tool success)
  - security principles (zero trust, least privilege, audit everything)

**Best for:** Explaining the platform to engineering leadership / enterprise stakeholders.

---

### 4) `evals-harness.md` — *Behavioral regression testing for agents*
**What it is:** A practical evals strategy: treat agent behavior like software by using **trace capture + replay** to prevent silent drift as models/prompts/tools change.

- Open: [evals-harness.md](./evals-harness.md)

**Highlights:**
- 3-layer testing approach:
  1. API contract tests (Jest + Supertest)
  2. Trace replay system (record → replay → diff)
  3. Assertion framework (schema validity, tool-policy compliance, routing invariants, latency budgets)
- Mermaid **pipeline diagram** for the evals flow
- Example test snippets for tool calling, spec compilation, and RAG retrieval

**Best for:** Teams shipping agents in production who need reliability and safety over time.

---

## Suggested reading order
1. **[system-architecture.md](./system-architecture.md)** (what the components are)
2. **[README.md](./README.md)** (how a single turn runs end-to-end)
3. **[architecture-by-planes.md](./architecture-by-planes.md)** (how it scales/operates in production)
4. **[evals-harness.md](./evals-harness.md)** (how you keep it from regressing)
