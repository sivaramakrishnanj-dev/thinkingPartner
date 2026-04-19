# Persistence

> *Stub.*

## v1 baseline
- Postgres (RDS in prod, Docker locally)
- pgvector extension for embeddings
- All core entities in the same DB

## Open question
Evaluate introducing **OpenSearch/Elasticsearch** for:
- Session / message search
- Knowledge pack full-text + hybrid search
- Operational observability on sessions

Pattern if adopted: references (ids, timestamps) in RDS, blobs / searchable text in OpenSearch. See [`../07-open-questions.md`](../07-open-questions.md) #1.
