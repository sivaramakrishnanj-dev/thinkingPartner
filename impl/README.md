# Implementation

Actual code lives here. Each buildable unit is self-contained with its own build system.

| Unit | Build | Status | Folder |
|---|---|---|---|
| Cloud service | Maven or Gradle (Spring Boot, Java 21) | v1 target | [`cloud-service/`](cloud-service/README.md) |
| Android client | Gradle (Kotlin + Compose) | v1 target | [`clients/android/`](clients/android/README.md) |
| Desktop Java client | TBD | Deferred to v2 | [`clients/desktop-java/`](clients/desktop-java/README.md) |

Design is in [`../design/`](../design/). Nothing in this folder references the design tree — one-way coupling from design → impl only.
