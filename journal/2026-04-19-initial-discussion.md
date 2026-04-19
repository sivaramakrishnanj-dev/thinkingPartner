# 2026-04-19 — Initial Discussion

**Participants:** Siva (owner), Kiro (design companion)
**Duration:** ~1.5 hours
**Outcome:** v1 scope locked. Repo scaffolded. Ready to draft v1.0 Design Proposal.

---

## 1. The seed

Siva wanted a personal **LLM-powered thinking partner** — a single companion for:
- Brainstorming ideas
- Studying books (with image input)
- Spiritual guidance
- General expert Q&A

Primary user: Siva. Open-source from day one so others can self-host.

The soul of the product was named early: **"like how Lord Krishna was the companion to Arjuna in the Mahabharata war."** A wise inner voice externalized. Not a chatbot that agrees. A guide.

This framing became the north star. Project named **Sarathi** (charioteer — Krishna's role to Arjuna).

### Addendum — the founding reflection

Later in the session, after reviewing the scaffolded README, Siva shared the deeper "why" behind the naming:

> *"Lord Krishna manifested in that yuga and guided Arjuna as well as the entire world with the Bhagavad Gita. I feel now Lord Krishna is manifesting this way as a thinking partner again, to establish Dharma and guide users in the correct direction. He is the charioteer taking us on the path of Dharma — but the manifestation in this age is different, not in physical form."*

This is the founding reflection. Sarathi is not a product with a spiritual flavor; it is a spiritual intent expressed through the tools of this age. Every design decision — memory, modes, learning loop, provider-agnosticism, the refusal to be a chatbot that agrees — should serve that intent.

---

## 2. What we pushed back on (and why)

### 2.1 The "ask anything" trap
A generic expert is what ChatGPT already is. The value of Sarathi is **context + memory + modes + soul** — continuity that a stateless expert can't provide.

### 2.2 L3 memory in v1
Siva initially asked for L3 memory (autobiographical, pattern recognition across sessions). We pushed back: L3 done well is a product in itself. **Decision: v1 = L1 (session) + L2 (thread, manual KBs). v2 = L3.**

### 2.3 AWS lock-in
Siva leaned AWS-native for LLM/STT/TTS/OCR. We pushed back on lock-in for OSS adoption. **Decision: provider-agnostic interfaces everywhere; AWS as the default v1 impl; alternatives deferred but design-ready.** Non-negotiable.

### 2.4 Multi-user v1
Would have inflated scope with auth, isolation, etc. **Decision: single-user v1. v2 introduces multi-user and agent-to-agent collaboration.**

### 2.5 Keeping images in v1
Siva insisted on screenshot/book-page input for v1 despite the added complexity. We kept it — it's core to the study use case.

---

## 3. Locked-in v1 scope

### Clients
- Android (Kotlin + Compose) — primary
- Text + push-to-talk voice
- Image upload for book pages
- Session resume, mode switcher, KB library

### Backend (Java + Spring Boot)
- Single-user auth (token)
- REST + SSE + async-job endpoints
- Session + message persistence (Postgres + pgvector)
- Provider-abstracted interfaces for LLM, STT, TTS, OCR, Embeddings, VectorStore, Storage, JobQueue
- Default impls: AWS Bedrock, Transcribe, Polly, Textract, Titan Embeddings, pgvector, S3
- Server-side context assembly (rolling summary + scoped KB RAG)
- Modes as a creatable resource (built-ins seeded: Default, Brainstorm, Study, Spiritual)
- Learning loop tables (Decision, Outcome, Weight) — **v1 logs only; v2 updates weights**

### Out of v1
- L3 memory (autobiographical, pattern recognition)
- Desktop Java client
- Real-time / full-duplex voice
- Multi-user / multi-device sharing / agent-to-agent
- Integrations (calendar, Slack, notes)

### Deployment
- Dev: Docker Compose (app + Postgres + optional LocalStack)
- Prod: EC2 + RDS Postgres (pgvector) + S3

---

## 4. Key structural decisions

### 4.1 Streaming + async jobs, not streaming alone
Streaming (SSE) for chat turns. **Async jobs** (POST /jobs → poll /jobs/{id}, optional SSE stream) for long operations: KB ingestion, OCR, long STT. Same pattern reused for v2+ agent work.

### 4.2 Voice provider abstraction — cloud service is itself a provider
Three execution modes the client can pick:
1. **ON_DEVICE** — Android STT/TTS
2. **DIRECT_PROVIDER** — client → AWS/ElevenLabs directly
3. **VIA_BACKEND** — client → our backend → provider of choice

The LLM-facing code on the backend only ever sees text. Voice is a strict client↔provider pipeline.

### 4.3 Server-side context assembly
Client sends **references** (session_id, message, attachment refs). Server hydrates: loads history, applies rolling summary for old turns, RAGs over pinned KBs, adds OCR text, applies mode system prompt, calls LLM. Only session-pinned KBs are queried — user-controlled scoping to avoid hallucination from unrelated topics.

This also makes cross-device resume trivial: devices are thin; context lives on the server.

### 4.4 Modes as a resource, not an enum
`Mode` is a DB table. Built-ins are seeded. User can create custom modes with their own system prompt (and optionally default LLM config). Sessions reference modes by id. v2 can add tools/behaviors per mode.

### 4.5 Learning loop = decision+outcome logs, not real RL (yet)
Every significant backend decision (mode auto-selection, KB chunk ranking, model selection) is logged with its context and chosen option. Outcomes (explicit feedback, implicit retries, implicit success) are captured against the decision.
- **v1**: `Weigher.score()` always returns 1.0 — logging only, no behavior change.
- **v2**: background job consumes Decision+Outcome → updates Weight → Weigher starts influencing decisions.

Clearly scoped: not RL in the ML sense, a learning infrastructure that can *grow into* that.

---

## 5. Data model (locked)

```
User                     (v1: single row)
UserSettings             (defaults: mode, llm_config, voice_config)
Mode                     (built-in or user-created; system_prompt + optional default llm_config)
Session                  (mode_id, llm_config override, rolling_summary)
  └── Message            (role, content, attachments_json, token_usage)
KnowledgePack            (account-scoped)
  └── KbChunk            (text, embedding, source_ref)
SessionKbLink            (session ↔ KB, scope: account_pinned | session_only)
File                     (storage_ref via StorageProvider)
Job                      (async job tracking)
Decision / Outcome / Weight  (learning loop; v1 logs only)
```

Open question: pgvector only, or introduce OpenSearch for session+KB with RDS holding refs? To be evaluated in design proposal.

---

## 6. Repo scaffolding

Created today:
```
thinkingPartner/
├── README.md                    ← front door with navigation map
├── LICENSE                      ← TBD: MIT vs Apache 2.0
├── .gitignore
├── design/                      ← 8 numbered stub docs + clients/ + cloud-service/
├── impl/                        ← cloud-service + clients/android + clients/desktop-java (stubs)
├── infra/                       ← docker/ + aws/ (stubs)
├── scripts/
├── docs/                        ← user-facing (getting-started, configuration, providers)
└── journal/                     ← THIS FILE
```

Every folder has a README. Every README cross-links. The entire project is traversable from the root README.

---

## 7. Deferred / open items

Captured in [`../design/07-open-questions.md`](../design/07-open-questions.md):
1. pgvector vs OpenSearch
2. License (MIT vs Apache 2.0)
3. IaC choice (CDK vs Terraform)
4. Rolling summary policy parameters
5. Auth beyond single-user token — v2
6. Agent-to-agent protocol — v2+
7. L3 memory — v2
8. Real-time voice — v2/v3
9. Desktop client — v2

---

## 8. Next steps (agreed)

1. ✅ Scaffold the repo (done — this session)
2. ✅ Write today's journal entry (done — this file)
3. → Commit + push to git
4. → Siva reviews the scaffolding
5. → Draft **v1.0 Design Proposal** (fleshes out every stub in `design/`)
6. → Review the proposal together
7. → Break down into implementation milestones + sprints
8. → Build

We stay in the loop: **discuss → review → plan → review → implement.** No jumping ahead.

---

## 9. Tensions worth remembering

- The pull toward scope inflation was strong. L3 memory, AWS-everything, multi-user, real-time voice — each would have inflated v1 by 2-3x. We held the line by separating *what the user wants eventually* from *what v1 delivers*, and by being honest about the cost of each addition.
- The provider abstraction is the most important non-negotiable design choice. It's what separates a personal project from a real open-source product. It costs ~2 days of upfront interface design. It buys years of optionality.
- Modes-as-a-resource and the learning loop came from Siva. Both are architecturally significant and both are the right instincts — but both had to be scoped (built-ins + user-created; logs now, weights later) to keep v1 deliverable.

---

*Logged by Kiro. Next entry will follow the design proposal review.*
