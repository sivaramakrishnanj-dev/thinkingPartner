# Agents — Vision and Open Design

> *Forward-looking document. Captures the v2+ vision for agents so it is not lost between v1 delivery and v2 design. This is not a specification; it is the shape of the thinking so far.*

---

## 1. Why agents matter to Sarathi

Sarathi's work relies most on an LLM — but **not only** on an LLM. To serve its responsibility as a wise companion, Sarathi must be able to reach outside itself: search the web, read a document, compute, fetch structured data, or delegate to another specialized agent. Principle 5 ("Continuity is the moat") makes memory the differentiator; agents are what make the companion *capable* beyond the confines of any single model's training data.

The architectural question is not *whether* Sarathi should use agents, but *how the seam should be shaped* so that:

- v1 delivers value with one in-process agent and zero new infrastructure
- v2 extends to out-of-process, polyglot, distributed agents without redesigning Sarathi's core
- v3+ opens the door to cross-instance agent collaboration between users' Sarathis

---

## 2. Where v1 stops

| Element | v1 reality |
|---|---|
| Agents available | One: `WebSearchAgent` (deterministic; no internal LLM) |
| Transport | In-process Java method call |
| Discovery | Static Spring bean registry (`InProcessAgentDirectory`) behind the `AgentDirectory` interface |
| Payload passing | Reference-based (File + StorageProvider) when large; inline when small |
| Authentication | None — single process, trusted |
| Observability | Logged via `learning` as `Decision(type=agent_invocation)` and regular app logs |

The `AgentDirectory` interface is the single seam. Swapping its implementation is how v2 arrives.

---

## 3. The v2 vision (sketch — not designed)

### 3.1 Event-driven agent ecosystem

Agents run as **independent processes**, possibly on different machines, possibly polyglot (Java, Python, Node), possibly authored by different people. They communicate with Sarathi — and each other — over an **event bus**.

```
Sarathi ── publishes "task requested" event ──▶  Event Bus
                                                    │
                               ┌────────────────────┼────────────────────┐
                               ▼                    ▼                    ▼
                         Agent A               Agent B               Agent C
                         (subscribes to        (subscribes to        (subscribes to
                          web_search)           summarize)            code_review)
                               │
                               ▼
                    publishes "task result" event ─▶  Event Bus  ─▶  Sarathi (consumes)
```

Payloads are passed **by reference**, not inline. A task event carries a task id and a pointer to the full payload (typically in S3 via `StorageProvider`). This keeps the bus cheap, the message small, and makes large-payload tasks (long documents, audio, batch images) natural.

### 3.2 Agent discovery

Not baked into the bus. A separate **Discovery Service** advertises agents:

- An agent registers with Discovery on startup: `{ agent_id, capabilities, version, endpoint/topic, owner, health, trust_level }`
- Agents emit heartbeats; stale registrations expire
- Sarathi queries Discovery at task time: "who can do `web_search`?" — receives candidate list
- Selection policy (round-robin, healthiest, cheapest, user-preferred, highest-trust) lives in Sarathi

In v2, Discovery may be part of the same instance. In v3+, Discovery itself may be shared across instances (federation).

### 3.3 Cross-instance collaboration (v3+)

The long-arc vision: one user's Sarathi can invoke an agent registered on another user's Sarathi (with explicit trust and consent). Use cases:

- A family shares a "household research" agent running on one member's instance
- Open-source communities publish reference agents that any Sarathi can use
- A user delegates a long-running task to a friend's Sarathi while their own is offline

This is not a v2 requirement. It's the reason we don't paint ourselves into a corner in v2.

---

## 4. Open design questions (to be settled when we build v2)

These are deliberately **not** decided now. They are listed so they are not forgotten.

### 4.1 Transport

- SQS + SNS — AWS native, low ops, fan-out via SNS topics
- EventBridge — richer routing, event schemas, but more AWS-specific
- Kafka — durable log, replayability, wins if the ecosystem grows; heavy for small deployments
- NATS — lightweight, subject-based, excellent latency; less AWS-integrated
- In-process JVM events (Disruptor / Spring Events) as a v1.5 step before going external

### 4.2 Event schema

- Required fields: `task_id`, `correlation_id`, `user_id`, `capability`, `input_ref`, `response_topic`, `deadline`, `emitted_at`
- Result fields: `task_id`, `status`, `result_ref` or `error`, `actual_cost?`, `completed_at`
- Versioning: schema version in every event; strict-but-forward-compatible evolution
- Serialization: JSON (human-readable) vs Avro/Protobuf (efficient, schema-enforced). JSON likely wins for the scale we will see.

### 4.3 Capability model

- How does an agent describe what it can do? A URI? A tag? A full JSON schema of its input/output?
- Can two agents advertise the same capability with different performance characteristics?
- Can capabilities be composite ("I can do web_search AND summarize in one call")?
- How does Sarathi reason about which capability to use?

### 4.4 Authentication and trust

- How does an agent prove it is who it says it is?
- Does Sarathi maintain a trust level per agent (built-in > trusted > untrusted > blocked)?
- Can a user approve/deny agents on their instance?
- What happens when an agent returns a response that tries to influence Sarathi's behavior (prompt injection, spoofed results)?
- Sandbox model for agents that execute code or access filesystems

### 4.5 Payload contract

- S3 paths: `s3://{instance-bucket}/tasks/{task_id}/input` and `.../output`
- Retention: tasks and payloads retained for N days, then garbage-collected
- Encryption: server-side at rest; in transit via TLS; optional payload-level encryption for multi-instance scenarios

### 4.6 Failure semantics

- Retries: idempotency required on agents — how do we enforce / detect?
- Timeouts: deadline in the event, agent must honor it, Sarathi must fail gracefully if exceeded
- Dead-letter queue handling
- Partial results (agent returns "I found some but not all")

### 4.7 Cost accounting

- Agents that cost money (LLM-backed, paid APIs) must report actual cost in the result event
- Usage flows back into `quota` as a deducted line item
- Multi-user shared-instance scenario: which user bore the cost?

### 4.8 Observability across agents

- End-to-end tracing: the `correlation_id` from a chat turn propagates through every agent call
- Per-agent metrics: latency, success rate, cost per invocation
- A single place to see "what did Sarathi do during this turn" — the turn narrative

### 4.9 Specific agents worth naming (post-v1 candidates)

These are agent types the v2 design should comfortably accommodate:

- `WebSearchAgent` (built-in, v1 — deterministic)
- `KiroCliAgent` — wrap the Kiro CLI as a full LLM-powered research agent
- `ClaudeCodeAgent` / `GeminiCliAgent` / etc. — same pattern, different upstream agent
- `DocumentSummarizerAgent` — ingests a document, returns structured summary
- `ScriptureRetrievalAgent` — specialized KB retrieval over curated spiritual corpora
- `CodeSearchAgent` — search public or private code repositories
- `CalendarReaderAgent` — read-only calendar context (personal integration, v3+ territory)
- `JournalRetrievalAgent` — look up the user's own past journal entries across sessions

No commitment. Just sketches of what the system should support without heroics.

---

## 5. The smallest possible v1.5 step toward v2

If we wanted a meaningful step between v1 and full v2, the smallest useful increment is:

1. Introduce Spring Events (in-process) as a transport — no external bus yet
2. Wrap `WebSearchAgent` to publish/consume through Spring Events instead of direct method calls
3. Add one more agent (candidate: `KiroCliAgent` or a document summarizer)
4. Exercise the payload-by-reference pattern for at least one large-payload case

That proves the event shape without infrastructure. The move from Spring Events to SQS/EventBridge becomes a transport swap, not an architectural change.

This is a possible v1.5 scope — not committed.

---

## 6. What this document is not

- Not a spec — no event schemas, no endpoint shapes
- Not a commitment — anything here may be revised when we start v2 design
- Not exhaustive — it captures what came out of the initial-discussion session on 2026-04-19 and what follows from it; more will emerge

This document is a way of honoring a good idea by preserving it at the right level of specificity — enough that we know what we meant, not so much that we pretend to have answered what we haven't.

---

*First drafted 2026-04-19 alongside [`01-overall-architecture.md`](01-overall-architecture.md). Expected to evolve as we approach v2 work.*
