# Open Questions

Running list of design items that are deliberately unresolved. Each will be settled in a design proposal or explicitly deferred to v2+.

| # | Question | Status | Notes |
|---|---|---|---|
| 1 | pgvector only, or introduce OpenSearch/Elasticsearch for session + KB data, with RDS holding references? | Open | Eval in `02-data-model.md`. pgvector wins for simplicity (no extra infra); OpenSearch wins on hybrid search + scale. |
| 2 | License: MIT or Apache 2.0? | Open | Decide before first public release. |
| 3 | Deployment IaC: AWS CDK or Terraform? | Open | Pick in [`cloud-service/deployment.md`](cloud-service/deployment.md). |
| 4 | Rolling summary policy: verbatim-turn count K, summarization cadence, summarizer model? | Open | Trade-off between cost, fidelity, latency. Design in `05-context-assembly.md`. |
| 5 | HTTPS ingress: self-managed cert on EC2, or ALB in front? | Open | Shapes `infra/aws/`. ALB adds cost but simplifies cert rotation. |
| 6 | Spring Boot version and Java version. | Deferred | Decide at implementation bootstrap. Lean: Spring Boot 3.x on Java 21 LTS. |
| 7 | Authentication beyond single-user token (identity, password hashing, invitation flow, admin role). | Deferred to v1.5/v2 | Data model already multi-user-ready. Implementation is single-user. |
| 8 | Event-bus transport for agents (SQS vs EventBridge vs Kafka vs NATS vs Spring Events in-process). | Deferred to v2 | See [`agents-vision.md`](agents-vision.md) §4.1. |
| 9 | Agent discovery service: standalone service vs embedded; capability schema; trust model. | Deferred to v2 | See [`agents-vision.md`](agents-vision.md) §3.2 and §4.3–§4.4. |
| 10 | Payload-by-reference contract (S3 key layout, retention, encryption at payload level). | Deferred to v2 | Pattern already in use in v1 via `File` + `StorageProvider`. Formalize with event bus. |
| 11 | Agent-to-agent cost accounting (which user pays when shared-instance agents cost money). | Deferred to v2 | See [`agents-vision.md`](agents-vision.md) §4.7. |
| 12 | L3 autobiographical memory (fact extraction, cross-session pattern recognition). | Deferred to v2 | Learning-loop logs in v1 lay the groundwork. |
| 13 | Learning-loop weight-update background job, `Weigher` behavior change. | Deferred to v2 | v1 logs only; `Weigher.score()` returns 1.0 in v1. |
| 14 | External log and metric sinks (CloudWatch, OpenSearch, S3 archive, Prometheus, Loki, Datadog, ...). | Deferred to v2 | Logback + Micrometer in v1 make these a config swap. |
| 15 | Real-time / full-duplex voice (ChatGPT-Voice-style). | Deferred to v2/v3 | v1 is push-to-talk with text responses (optional TTS playback). |
| 16 | Desktop Java client. | Deferred to v2 | Placeholder in [`clients/desktop-java/`](clients/desktop-java/). |
| 17 | Cross-instance Sarathi federation (my Sarathi invoking agents registered on another user's Sarathi). | Deferred to v2+ | See [`agents-vision.md`](agents-vision.md) §3.3. Design must not preclude. |
| 18 | Public signup / open registration. | **Not planned** | Shared-instance mode in v2 is invitation-only by design. |
| 19 | Per-user provider credentials (as opposed to instance-level shared credentials). | Deferred to v2+ | v2 shared-instance uses instance-level credentials; per-user is a later enhancement if needed. |
