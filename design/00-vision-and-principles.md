# Vision & Principles

> *Stub. To be drafted in the v1.0 Design Proposal.*

## What Sarathi is

A personal, cross-device, LLM-powered thinking partner — a unified wise companion that adapts tone based on context (brainstorm / study / spiritual / default / user-defined modes). Inspired by Krishna's role as Arjuna's sarathi (charioteer) in the Mahabharata: a guide, not a servant; a mirror, not an echo.

## What Sarathi is not

- Not a chatbot that agrees with you
- Not a task manager
- Not a search engine
- Not a productivity tool

## Guiding principles

- **Provider-agnostic** — every external capability (LLM, STT, TTS, OCR, Vector Store, File Storage) is behind a swappable interface. Default impls are AWS. OSS users can swap.
- **Server-assembled context** — clients send references, server hydrates. Devices are thin.
- **Memory layered** — v1: session + thread. v2: autobiographical (L3).
- **Modes as a resource** — users can create their own modes, not just pick from built-ins.
- **Learning loop from day one** — decisions + outcomes are logged even in v1, so v2 can train on them.
- **Open-source from day one** — each user self-hosts their own instance.
- **Single-user v1** — multi-user and agent-to-agent collaboration is v2+.
