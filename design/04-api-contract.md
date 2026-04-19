# API Contract

> *Stub. To be drafted in the v1.0 Design Proposal.*

The backend exposes three kinds of endpoints:

### 1. Synchronous REST (CRUD)
Sessions, Messages (list/get), Modes, KnowledgePacks, UserSettings, Files (upload/download signed URL).

### 2. Streaming (SSE)
- `POST /sessions/{id}/turns` with `Accept: text/event-stream` → streams assistant tokens.
- `GET /jobs/{id}/stream` → SSE-push job status updates.

### 3. Async jobs
- `POST /jobs` (type, input ref) → `{ job_id, status: pending }`
- `GET  /jobs/{id}` → `{ status, result?, error? }`
- Used for: KB ingestion, OCR, long STT, any future multi-step agent work.

Auth: single-user token for v1.

Full OpenAPI spec: TBD.
