---
type: architecture
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/architecture
  - pkm/domain/architecture
---

# Architecture

## Objective
Explain the system boundaries, control flow, data contracts, and failure handling that make realtime persona sessions work.

## Description
### What
The architecture is a layered realtime product system. A frontend surface asks for a session, the central API validates who may start it, Supabase atomically reserves runtime capacity, LiveKit carries media/control traffic, and either a third-party or first-party voice agent produces audio that drives avatar synthesis.

### Why
The layers separate trust boundaries. Browser code can hold publishable keys and short-lived tokens, but not raw provider API keys. The API can see org/billing/persona state, but does not run the realtime turn loop. Supabase owns atomic capacity decisions because those must remain correct under concurrent requests. LiveKit and WebSockets carry media because they are designed for low-latency bidirectional streams.

### When
This architecture appears whenever a user publishes an embed, starts a session from the SDK, runs a dashboard preview, or creates a KFL voice-agent session. It also appears indirectly in billing and cleanup jobs because reservations are the common runtime accounting object.

### How
A canonical embedded session works like this:
1. Browser calls `/v1/embed/create_session` with a publishable key.
2. API hashes the key, loads only active configs, validates the request origin, resolves plan and billing eligibility, and verifies the persona is still visible.
3. API mints short-lived provider credentials, or for KFL waits until reservation creation so the signed WebSocket URL can include reservation-bound claims.
4. API reserves GPU capacity with Supabase RPCs, creates a LiveKit room, mints user and agent room tokens, and dispatches the avatar worker to the selected node.
5. Browser connects the Persona session to LiveKit, connects the voice agent, streams microphone PCM into the agent, and forwards agent PCM to the avatar over LiveKit byte streams.

## Scope
| Boundary | Responsibility | Primary Failure Mode |
| --- | --- | --- |
| SDK layer | LiveKit room connection, byte-stream audio, control data, media callbacks | Topic or token mismatch |
| Elements layer | DOM media elements, microphone capture, voice-agent adapter normalization | Browser autoplay, mic permissions, provider disconnect |
| Central API | Auth, billing, persona resolution, session creation, secret handling | Incorrect admission or leaked credential |
| Supabase | RLS, publishable config state, reservation RPCs, metering state | Race conditions or stale active reservations |
| KFL voice agent | STT -> LLM -> TTS orchestration and interruption semantics | Turn ordering, partial-audio memory, provider failure |
| Tests | Behavioral documentation for edge cases | Coverage drift from product behavior |

## Use Case
Use this note to understand cross-boundary behavior before editing any workflow that starts sessions, modifies embed configs, changes voice-agent adapters, or touches reservations/billing.

## Stage In Lifecycle
Runtime and design. It explains both live execution and architectural intent.

## Importance
Core. Nearly every meaningful feature crosses package, API, database, and runtime boundaries.

## Risks
- Realtime systems fail at edges: timing, cancellation, partial streams, duplicate connections, provider outages, and browser media policy.
- The same concept can exist in several forms: dashboard config, persisted JSONB, API response model, signed URL claim, browser adapter config, and runtime struct.
- Reservation cleanup is safety-critical because an unreleased reservation means lost capacity and inaccurate billing.
- Research assumptions can leak into product behavior through emotion labels, representation formats, or sample rates.

## Input / Output
| Input | Output |
| --- | --- |
| Dashboard publish or SDK/embed session request | Session credentials, voice-agent credentials, reservation row, LiveKit room |
| Browser microphone PCM | Voice-agent request stream and avatar audio/video behavior |
| Runtime reservation lifecycle | Billing credits, concurrency availability, cleanup actions |

## Resources
- [[Concepts/Voice AI Pipeline]]
- [[Definitions/Database RPC]]
- [[Workflows/Real-Time Persona Session]]
- [[Workflows/Real-Time Persona Session]]
    - [[Workflows/Embed Config Builder Lifecycle]]
    - [[Workflows/Billing Usage And Session Limits]]
    - [[Workflows/KFL Voice Agent Session]]
    - [[Concepts/Reservation And Node Capacity]]
    - [[Concepts/LiveKit Transport]]
    - [[Concepts/Research Source Index]]

    External references:
    - W3C WebRTC specification: https://www.w3.org/TR/webrtc/
- WebSocket protocol RFC 6455: https://www.rfc-editor.org/rfc/rfc6455
- LiveKit realtime media docs: https://docs.livekit.io/transport/media/
- LiveKit realtime data docs: https://docs.livekit.io/transport/data/
- FastAPI documentation: https://fastapi.tiangolo.com/
- PostgreSQL row security: https://www.postgresql.org/docs/current/ddl-rowsecurity.html
- Supabase RLS guide: https://supabase.com/docs/guides/database/postgres/row-level-security
- Stripe usage-based billing docs: https://docs.stripe.com/billing/subscriptions/usage-based/recording-usage-api
- OpenAI Realtime guide: https://platform.openai.com/docs/guides/realtime
- PyTorch paper: https://papers.neurips.cc/paper/9015-pytorch-an-imperative-style-high-performance-deep-learning-library
- WavLM paper: https://arxiv.org/abs/2110.13900
- Wav2Lip paper: https://arxiv.org/abs/2008.10010
- EMO paper: https://arxiv.org/abs/2402.17485

## Related Topics
- [[Definitions/LiveKit Agent]]
- [[Definitions/Race Condition]]
- [[Philosophies/Layered SDK Abstraction]]
- [[Philosophies/Layered SDK Abstraction]]
- [[Philosophies/Publishable-Key Embed Model]]
- [[Philosophies/Reservation-Based GPU Capacity]]
- [[Philosophies/Real-Time Streaming Pipeline]]
- [[Technologies/LiveKit]]
- [[Technologies/Supabase]]
- [[Technologies/WebRTC]]
- [[Technologies/WebSockets]]

## Evidence
- js-sdk/src/PersonaSession.ts
- js-elements/src/PersonaEmbed.ts
- js-elements/src/agents/types.ts
- modal_kflapi/app.py
- modal_common/agents.py
- supabase-kfl/supabase/migrations/20260109164203_nodes_and_reservations_rpcs.sql
- supabase-kfl/supabase/migrations/20260124212220_embed_configs.sql
- supabase-kfl/supabase/migrations/20260414120000_embed_config_drafts.sql
- platform/src/features/agents/useWidgetBuilderStore.ts
- kfl-voice-agent/ARCHITECTURE.md
- kfl-voice-agent/internal/voiceagent/session_core.go
- kfl-voice-agent/internal/voiceagent/stt.go
- kfl-voice-agent/internal/voiceagent/llm.go
- kfl-voice-agent/internal/voiceagent/tts.go
