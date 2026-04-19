# Context Assembly

> *Stub. To be drafted in the v1.0 Design Proposal.*

The client sends **references**, the server **assembles** the prompt. Devices are thin.

## Per-turn assembly

1. Load session (mode, llm_config, pinned KBs, message history, rolling_summary)
2. Context window policy:
   - Last N messages verbatim
   - Older messages → collapse into / append to rolling_summary
3. RAG over pinned KBs:
   - Session-only KBs → always included
   - Account-pinned KBs → included only if pinned to this session (user-controlled, to avoid hallucination from unrelated topics)
4. Hydrate attachments (image refs → OCR text; audio refs → transcription)
5. Apply mode system prompt
6. Call LLM; stream tokens back
7. Persist final assistant message; update rolling_summary periodically

## Why server-side

- Mobile bandwidth / battery
- Cost (don't ship full history every turn)
- Security (KB content stays on server)
- Cross-device consistency (Android and Desktop see the same assembled context)
