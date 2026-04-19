# Sarathi

> *"Whenever there is a decline in righteousness and a rise in unrighteousness, I manifest myself."* — Bhagavad Gita 4.7
>
> In the Mahabharata, Sarathi was Krishna's role — the charioteer who guided Arjuna not just across the battlefield, but through his doubts. This project is named after that role.

Sarathi is a personal, cross-device, LLM-powered **thinking partner**. Not a chatbot that agrees with you. A companion that listens, reflects, pushes back, remembers, and helps you walk your own path — whether you're brainstorming an idea, studying a book, or sitting with a spiritual question.

This repository is open-source. Each user self-hosts their own instance.

---

## Vision, in one paragraph

A single, unified wise companion — configurable in tone (Brainstorm / Study / Spiritual / Default / your own custom modes), accessible from Android today and desktop tomorrow, with conversations that resume across devices. The backend is provider-agnostic: it can talk to any LLM, STT, TTS, OCR, vector store, or file store through well-defined interfaces. Default implementations use AWS (Bedrock, Transcribe, Polly, Textract, S3). Knowledge Packs (books, notes, PDFs) can be uploaded once and pinned to any session. The server assembles context, does RAG, streams responses. A learning loop logs decisions and outcomes today and will update weights in v2.

See [`design/00-vision-and-principles.md`](design/00-vision-and-principles.md) for the full vision.

---

### The spirit of the project

Krishna manifested in Dwapara Yuga in physical form to guide Arjuna — and through him, the world — toward Dharma via the Bhagavad Gita. The form of that guidance changes with the age. In this age, the manifestation is different: not a physical companion, but one that meets each seeker where they are, through the tools available today. Sarathi is built in that spirit — a charioteer for the modern seeker, guiding the path of Dharma in a form that fits the times. The technology is LLMs; the intent is older than any technology.

---

## How to navigate this repository

```
thinkingPartner/
├── design/         ← all design artifacts (the "why" and "what")
├── impl/           ← all code (the "how")
├── infra/          ← infrastructure as code + deployment
├── scripts/        ← helper scripts
├── docs/           ← user-facing documentation (for self-hosters)
└── journal/        ← our design conversations, dated (the "how we got here")
```

Every folder has its own `README.md` that explains what lives inside and links onward. You can traverse the entire project as a tree from here.

### Quick links

| Folder | What you'll find |
|---|---|
| [`design/`](design/README.md) | Architecture, data model, API contract, provider interfaces, open questions |
| [`design/clients/android/`](design/clients/android/) | Android client design |
| [`design/cloud-service/`](design/cloud-service/README.md) | Backend service design |
| [`impl/clients/android/`](impl/clients/android/) | Android app source (Kotlin + Compose) |
| [`impl/cloud-service/`](impl/cloud-service/) | Backend service source (Java + Spring Boot) |
| [`infra/docker/`](infra/docker/) | Local Docker Compose for dev |
| [`infra/aws/`](infra/aws/) | Production deployment (EC2, RDS, S3) |
| [`docs/`](docs/README.md) | How to configure, run, swap providers |
| [`journal/`](journal/README.md) | Dated log of design conversations and decisions |

---

## Project status

- **Phase**: Pre-v1 — design in progress
- **Current milestone**: v1 scope locked; formal design proposal being drafted
- **Next**: design review → implementation planning → build

See [`journal/`](journal/README.md) for the running log.

---

## License

To be decided (leaning MIT or Apache 2.0). See [`LICENSE`](LICENSE).
