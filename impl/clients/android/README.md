# Sarathi Android Client

Kotlin + Jetpack Compose.

**Status:** Not yet bootstrapped.

Design: [`../../../design/clients/android/`](../../../design/clients/android/README.md)

## Planned layout (subject to design proposal)
```
android/
├── app/
│   ├── build.gradle.kts
│   └── src/main/kotlin/com/sarathi/
│       ├── SarathiApp.kt
│       ├── ui/             ← Compose screens
│       ├── data/           ← Repository, DTOs
│       ├── net/            ← HTTP + SSE clients
│       ├── voice/          ← STT/TTS configurable layer
│       └── di/
└── settings.gradle.kts
```
