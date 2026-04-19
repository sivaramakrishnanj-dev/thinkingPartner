# Data Model

> *Stub. To be drafted in the v1.0 Design Proposal.*

## Entities (v1, agreed in initial discussion)

- **User** — single row in v1
- **UserSettings** — default LLM config, voice config, default mode
- **Mode** — creatable resource; built-ins seeded (Default, Brainstorm, Study, Spiritual)
- **Session** — one conversation thread; carries mode, llm_config, rolling_summary
- **Message** — role, content, attachments_json, token_usage
- **KnowledgePack** — account-scoped uploaded document
- **KbChunk** — text + embedding + source ref
- **SessionKbLink** — many-to-many with scope (account_pinned | session_only)
- **File** — audio/image/doc blobs (in Storage provider, not DB)
- **Job** — async job tracking (ingestion, OCR, long STT)
- **Decision / Outcome / Weight** — learning-loop tables (v1 logs only; v2 updates weights)

Details, DDL, and index strategy: TBD in design proposal.

## Open: Postgres + pgvector only, or introduce OpenSearch for sessions/KB?

See [`07-open-questions.md`](07-open-questions.md).
