# Repo Summary (OpenAI Builder Lounge)

This repo is a compact set of architecture notes for an agent runtime (ChatAndBuild) with a focus on **observable, secure, policy-gated tool use**, plus an **evals harness** to prevent behavioral regressions.

## What’s in here

### `readme.md`
**Purpose:** High-level overview of the repo and how the docs fit together.

**What you’ll find:**
- A “map” of the documents
- Pointers to the main architecture views
- A lightweight entry point for readers

**When to read:** First.

---

### `system-architecture.md`
**Purpose:** Systems view organized into **four planes** (Control, Data, Event, Session).

**What you’ll find:**
- A mermaid diagram showing the planes and how they interact
- Responsibilities for each plane (API/orchestration, storage/RAG, streaming/jobs/observability, realtime voice)
- Security and scaling notes (zero trust, least privilege, audit trails)

**When to read:** When you want the big picture of infrastructure and boundaries.

---

### `architecture-by-planes.md`
**Purpose:** A deeper “enterprise” version of the four-plane architecture, with more operational detail.

**What you’ll find:**
- Detailed breakdown of each plane’s components and responsibilities
- Data plane specifics (MongoDB collections, vector index approach)
- Event plane specifics (Socket.IO streaming, BullMQ/Redis jobs, metrics)
- Session plane specifics (LiveKit + OpenAI RealtimeModel + optional avatar)
- Operational patterns (monitoring, CI/CD, disaster recovery)

**When to read:** When you’re designing deployment, ops, or security posture.

---

### `evals-harness.md`
**Purpose:** How to test agent behavior like software using **trace replay** and invariant-based assertions.

**What you’ll find:**
- Why “vibe checks” fail and why evals are required
- A testing architecture: API contract tests + trace replay + assertion framework
- Invariants to protect (schema validity, tool-policy compliance, routing, latency)
- Sample Jest-style tests and an evals pipeline diagram

**When to read:** When you’re shipping agents and need regression protection.

---

## Suggested reading order
1. `readme.md`
2. `system-architecture.md`
3. `architecture-by-planes.md`
4. `evals-harness.md`

## Quick notes / opportunities
- Consider renaming `readme.md` → `README.md` so GitHub renders it as the default landing page.
- If you want this repo to be more “navigable,” adding a short **Table of Contents** and cross-links between docs would help.

