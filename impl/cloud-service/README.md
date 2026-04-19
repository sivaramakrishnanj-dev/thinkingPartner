# Cloud Service (Implementation)

Spring Boot / Java 21 backend.

**Status:** Not yet bootstrapped. Scaffolding will follow the v1.0 Design Proposal and implementation plan.

Design: [`../../design/cloud-service/`](../../design/cloud-service/README.md)

## Planned layout (subject to design proposal)

```
cloud-service/
├── pom.xml or build.gradle.kts
├── src/main/java/.../
│   ├── SarathiApplication.java
│   ├── api/            ← REST + SSE controllers
│   ├── chat/           ← turn orchestration
│   ├── context/        ← context assembly
│   ├── mode/           ← mode CRUD + seed
│   ├── kb/             ← knowledge pack ingestion + query
│   ├── voice/          ← STT/TTS façade
│   ├── providers/      ← provider interfaces + AWS impls
│   ├── jobs/           ← async job queue
│   ├── learning/       ← decision/outcome logging (stub v1)
│   ├── persistence/    ← JPA entities + repos
│   ├── storage/        ← file storage façade
│   ├── auth/           ← single-user token
│   └── config/
└── src/main/resources/
    ├── application.yml
    └── db/migration/   ← Flyway or Liquibase
```
