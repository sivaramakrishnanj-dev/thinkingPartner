# KB Ingestion Pipeline

> *Stub.*

Ingestion is always async (POST → job_id → poll/SSE).

## Steps
1. Upload file → `StorageProvider.put()` → file_id
2. `POST /kb` with file_id, name, source_type → returns job_id
3. Worker:
   - Extract text (plain / PDF / OCR via `OcrProvider` if image-only)
   - Chunk (target ~500 tokens, overlap ~50)
   - Embed chunks via `EmbeddingProvider`
   - Write to `KbChunk` + `VectorStore`
   - Mark KB `READY`
4. Client polls `GET /jobs/{id}` or subscribes to `/jobs/{id}/stream`

## Query
At chat turn time, context assembler calls `VectorStore.query(kb_ids, query_embedding, topK)` and hydrates chunk text into the prompt. Only KBs **pinned to the session** are queried (user-controlled scoping to avoid hallucination from unrelated topics).
