# Provider Interfaces

> *Stub. To be drafted in the v1.0 Design Proposal.*

Every external capability is a swappable interface. v1 default impls are AWS-based.

| Interface | v1 default | Alternative impls (future) |
|---|---|---|
| `LlmProvider` | AWS Bedrock (Claude Sonnet default, swappable per-session) | Anthropic direct, OpenAI, Ollama |
| `SttProvider` | AWS Transcribe | On-device (Android), OpenAI Whisper, local Whisper |
| `TtsProvider` | AWS Polly | ElevenLabs, on-device, Coqui |
| `OcrProvider` | AWS Textract | Tesseract local, vision-LLM (Claude/GPT-4o) |
| `EmbeddingProvider` | Bedrock Titan Embeddings | OpenAI embeddings, local (bge, etc.) |
| `VectorStore` | pgvector (inside RDS Postgres) | OpenSearch, Chroma (local) |
| `StorageProvider` | AWS S3 | Local filesystem |
| `JobQueue` | In-process (Spring @Async + DB-backed Job table) | SQS + separate worker |

### The voice twist

The cloud service **is itself** a voice provider — clients can pick:
1. **On-device** — Android STT / TTS directly
2. **Direct-provider** — client talks to AWS/ElevenLabs/etc. with its own creds
3. **Via-backend** — client talks to our service, service picks a provider

See [`05-context-assembly.md`](05-context-assembly.md) for how these flows plug in.
