# Android Client Design

> *Stub. To be drafted in the v1.0 Design Proposal.*

## Scope (v1)

- Kotlin + Jetpack Compose
- Screens: Session list, Session view (chat), Mode picker, Settings, KB library
- Text input + push-to-talk voice
- Image upload (camera / gallery) for book pages
- SSE streaming of assistant responses
- Voice config selector: on-device | direct-provider | via-backend
- Settings: backend URL, auth token, default LLM, default mode

## Not in v1

- Real-time full-duplex voice
- Offline mode
- Widgets / share-sheet integration

## Architecture (outline)

- MVVM with a single Repository layer that talks to the backend
- SSE client for streaming turns
- Background upload worker for KB ingestion and long jobs (polls via `/jobs/{id}`)

Detailed breakdown: TBD.
