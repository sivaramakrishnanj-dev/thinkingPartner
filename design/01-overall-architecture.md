# Overall Architecture

> *Stub. To be drafted in the v1.0 Design Proposal.*

Will cover:

- System diagram (clients ↔ backend ↔ providers)
- Component decomposition of the cloud service
- Request flows:
  - Synchronous chat turn (client → SSE stream from backend → LLM provider → back)
  - Async job (client POSTs job → polls or SSE-subscribes → result)
  - Knowledge Pack ingestion
  - Voice in/out (three configurable modes: on-device, direct-provider, via-backend)
- Deployment topology (local Docker dev → EC2 + RDS + S3 prod)

See also:
- [`02-data-model.md`](02-data-model.md)
- [`03-provider-interfaces.md`](03-provider-interfaces.md)
- [`04-api-contract.md`](04-api-contract.md)
- [`cloud-service/`](cloud-service/README.md)
