# Desktop Java Client

**Status: Deferred to v2.**

Rationale: the [API contract](../../04-api-contract.md) is designed to be client-agnostic. Once the Android client is working against a stable backend, the desktop client is largely a port: same API, same SSE streaming, same session-resume semantics. Likely JavaFX or Compose Multiplatform — decision deferred.

Cross-device session resume is a v1 property of the backend, not a client feature. Any new client that speaks the API contract gets resume for free.
