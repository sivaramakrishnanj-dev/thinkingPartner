# Design

This folder holds all design artifacts for Sarathi — the "why" and "what" of the system. Implementation lives in [`../impl/`](../impl/).

Design is organized top-down: start with vision, then overall architecture, then per-component design.

---

## Top-level documents

| Doc | Purpose |
|---|---|
| [`00-vision-and-principles.md`](00-vision-and-principles.md) | What Sarathi is, what it isn't, guiding principles |
| [`01-overall-architecture.md`](01-overall-architecture.md) | System diagram, components, data flow |
| [`02-data-model.md`](02-data-model.md) | Entities, relationships, persistence shape |
| [`03-provider-interfaces.md`](03-provider-interfaces.md) | LLM, STT, TTS, OCR, VectorStore, Storage abstractions |
| [`04-api-contract.md`](04-api-contract.md) | REST + SSE + async job endpoints between clients and backend |
| [`05-context-assembly.md`](05-context-assembly.md) | How the backend builds the LLM prompt (memory, KB RAG, summaries) |
| [`06-learning-loop.md`](06-learning-loop.md) | Decision + outcome logging (v1 stub, v2 weight updates) |
| [`07-open-questions.md`](07-open-questions.md) | Unresolved design items (e.g., pgvector vs OpenSearch) |
| [`agents-vision.md`](agents-vision.md) | Forward-looking vision for v2+ agent ecosystem (event bus, discovery, cross-instance) |

## Per-component design

| Area | Where |
|---|---|
| Android client | [`clients/android/`](clients/android/) |
| Desktop Java client (deferred) | [`clients/desktop-java/`](clients/desktop-java/) |
| Cloud service (backend) | [`cloud-service/`](cloud-service/README.md) |

---

## Status

All docs in this folder are currently **stubs**. The v1.0 Design Proposal will be drafted next. See [`../journal/`](../journal/) for the running record of design conversations.
