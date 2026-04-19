# Cloud Service Design

The backend is a Java / Spring Boot service. It is the stateful brain: it owns sessions, messages, knowledge packs, decisions, files (by reference), and all provider integrations.

## Documents

| Doc | Purpose |
|---|---|
| [`module-breakdown.md`](module-breakdown.md) | Internal modules and their responsibilities |
| [`persistence.md`](persistence.md) | Postgres schema, pgvector usage, OpenSearch eval |
| [`voice-pipeline.md`](voice-pipeline.md) | How STT/TTS flow through the service (via-backend mode) |
| [`kb-ingestion-pipeline.md`](kb-ingestion-pipeline.md) | Upload → chunk → embed → index → query |
| [`deployment.md`](deployment.md) | Docker dev, EC2/RDS/S3 prod, IaC choice |

All currently stubs — to be drafted in the v1.0 Design Proposal.
