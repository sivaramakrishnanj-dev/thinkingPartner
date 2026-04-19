# Open Questions

Running list of design items that are deliberately unresolved. Each will be settled in the design proposal or explicitly deferred to v2+.

| # | Question | Status | Notes |
|---|---|---|---|
| 1 | pgvector only, or introduce OpenSearch/Elasticsearch for session + KB data, with RDS holding references? | Open | Eval in v1.0 design proposal. pgvector is simpler (no extra infra); OpenSearch wins on hybrid search + scale. |
| 2 | License: MIT or Apache 2.0? | Open | Decide before first public release. |
| 3 | Deployment IaC: AWS CDK or Terraform? | Open | Pick in [`cloud-service/deployment.md`](cloud-service/deployment.md). |
| 4 | Rolling summary policy: how many verbatim turns (K), summarization cadence, summarizer model? | Open | Trade-off between cost, fidelity, latency. |
| 5 | Authentication beyond single-user token (OIDC, device pairing)? | Deferred to v2 | Single-user in v1. |
| 6 | Agent-to-agent protocol (sharing knowledge/actions between users' Sarathis). | Deferred to v2+ | Design should not preclude it. |
| 7 | L3 memory (autobiographical, pattern recognition across sessions). | Deferred to v2 | Logging lays groundwork. |
| 8 | Real-time voice (full-duplex like ChatGPT Voice Mode). | Deferred to v2/v3 | v1 is push-to-talk. |
| 9 | Desktop Java client. | Deferred to v2 | Placeholder in [`clients/desktop-java/`](clients/desktop-java/). |
