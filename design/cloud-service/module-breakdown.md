# Module Breakdown (Cloud Service)

> *Stub.* Proposed top-level modules:

- **api** — REST controllers, SSE endpoints, async job endpoints
- **chat** — session orchestration, turn handling, streaming
- **context** — context assembly (history + summary + KB RAG + attachments)
- **mode** — mode CRUD, built-in seeding
- **kb** — knowledge pack ingestion + query
- **voice** — STT/TTS façade (when acting as a voice provider)
- **providers** — provider interfaces + default AWS impls
- **jobs** — async job queue, workers
- **learning** — Decision/Outcome logging, Weigher (stub in v1)
- **persistence** — JPA entities, repositories
- **storage** — file storage façade (S3 / local fs)
- **auth** — single-user token auth (v1)
- **config** — application configuration, provider selection
