# Voice Pipeline

> *Stub.*

The backend is **one of three** places voice can run. Config lives on the client.

## via-backend STT flow
1. Client uploads audio blob → `POST /files` returns file_id
2. Client calls `POST /voice/stt` with file_id (short clip, sync) or creates a job (long clip, async)
3. Backend selects `SttProvider` impl (default AWS Transcribe); returns text

## via-backend TTS flow
1. Client calls `POST /voice/tts` with text
2. Backend selects `TtsProvider` impl (default AWS Polly); returns audio file reference
3. Client downloads audio via signed URL

## The LLM-facing layer sees only text
Voice is strictly a client↔provider pipeline; the chat/context modules never handle audio.
