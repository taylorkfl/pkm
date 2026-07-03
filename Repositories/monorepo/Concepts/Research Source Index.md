---
type: concept
repo: monorepo
status: growing
lifecycle_stage: research
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/research
---

# Research Source Index

## Objective
Collect external articles, standards, official docs, and papers that support deeper reasoning in the monorepo PKM.

## Description
### What
This is the vault’s external source map. It separates repo evidence from outside references and helps each technology/philosophy note ground claims in primary docs, standards, or research papers.

### Why
A Wikipedia-style repo PKM needs evidence beyond code when explaining protocols, billing patterns, realtime media, security boundaries, or ML/research assumptions. External sources should support claims, not replace local evidence.

### When
Use this note before adding claims like “WebRTC is appropriate for low-latency media,” “RLS enforces row visibility,” “usage billing is reported asynchronously,” or “speech/lip-sync models preserve certain timing signals.”

### How
Prefer official docs for technologies and peer-reviewed/preprint papers for research claims. In individual notes, cite only the sources relevant to that note and connect the claim back to repo evidence.

## Scope
| Source Type | Use For | Examples |
| --- | --- | --- |
| Standards/RFCs | Protocol behavior and constraints | WebRTC spec, WebSocket RFC |
| Official docs | Product/service behavior | LiveKit, FastAPI, Supabase, Stripe, OpenAI, ElevenLabs |
| Papers | Research mechanisms and model assumptions | PyTorch, WavLM, Wav2Lip, EMO |
| Repo evidence | What this code actually does | Source files, migrations, tests |

## Use Case
Use when deepening notes or checking whether a claim should be phrased as repo fact, external fact, or inference.

## Stage In Lifecycle
Research and discovery. It is a source map for the rest of the vault.

## Importance
High. It keeps deeper notes grounded and prevents invented certainty.

## Risks
- External docs can change; prefer durable docs and include exact repo evidence for local behavior.
- Papers support possibilities and methods, not necessarily what this repo already implements.
- Avoid source sprawl; add sources because they support a specific codebase explanation.
- Do not let citations substitute for reading migrations, tests, and runtime code.

## Input / Output
Not applicable

## Resources
Protocol and realtime:
- W3C WebRTC specification: https://www.w3.org/TR/webrtc/
- WebSocket protocol RFC 6455: https://www.rfc-editor.org/rfc/rfc6455
- WebRTC security architecture RFC 8827: https://www.rfc-editor.org/rfc/rfc8827
- LiveKit realtime media docs: https://docs.livekit.io/transport/media/
- LiveKit realtime data docs: https://docs.livekit.io/transport/data/
- LiveKit Keyframe avatar plugin docs: https://docs.livekit.io/agents/models/avatar/plugins/keyframe/

API, persistence, billing, and security:
- FastAPI documentation: https://fastapi.tiangolo.com/
- Pydantic models: https://docs.pydantic.dev/latest/concepts/models/
- Modal documentation: https://modal.com/docs
- PostgreSQL row security: https://www.postgresql.org/docs/current/ddl-rowsecurity.html
- Supabase RLS guide: https://supabase.com/docs/guides/database/postgres/row-level-security
- Stripe usage-based billing docs: https://docs.stripe.com/billing/subscriptions/usage-based/recording-usage-api
- OWASP API Security Project: https://owasp.org/API-Security/

Voice/AI providers:
- OpenAI Realtime guide: https://platform.openai.com/docs/guides/realtime
- ElevenLabs technology note: [[Technologies/ElevenLabs]]
- ElevenAgents WebSocket docs: https://elevenlabs.io/docs/eleven-agents/libraries/web-sockets
- ElevenAgents post-call webhooks: https://elevenlabs.io/docs/eleven-agents/workflows/post-call-webhooks
- Soniox realtime STT docs: https://soniox.com/docs/stt/rt/overview
- OpenRouter streaming docs: https://openrouter.ai/docs/api-reference/streaming
- Cartesia docs: https://docs.cartesia.ai/

Frontend and build:
- React Learn docs: https://react.dev/learn
- TypeScript Handbook: https://www.typescriptlang.org/docs/handbook/intro.html
- Vite guide: https://vite.dev/guide/
- MDN custom elements guide: https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_custom_elements

Research and ML:
- PyTorch paper: https://papers.neurips.cc/paper/9015-pytorch-an-imperative-style-high-performance-deep-learning-library
- WavLM paper: https://arxiv.org/abs/2110.13900
- Wav2Lip paper: https://arxiv.org/abs/2008.10010
- EMO paper: https://arxiv.org/abs/2402.17485
- Attention Is All You Need: https://arxiv.org/abs/1706.03762

## Related Topics
- [[Architecture]]
- [[Workflows/S1 S2 Research Plan]]
- [[Technologies/Technology Index]]
- [[Philosophies/Philosophy Index]]
- [[Concepts/Concept Index]]

## Evidence
- This note intentionally uses external URLs as supporting resources.
- Repo-grounded claims should still cite repo-relative evidence in the individual target notes.
