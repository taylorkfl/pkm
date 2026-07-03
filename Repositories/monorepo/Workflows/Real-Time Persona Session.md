---
type: workflow
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/workflow
  - pkm/domain/runtime
---

# Real-Time Persona Session

## Objective
Explain the complete runtime path from a session request to LiveKit media, voice-agent audio, avatar output, interruptions, cleanup, and usage accounting.

## Description
### What
A realtime persona session is the core product workflow. It admits a caller, resolves a persona, reserves GPU capacity, creates a LiveKit room, dispatches an avatar worker, connects browser media, and streams audio/control data between the browser, voice agent, and avatar runtime.

### Why
The flow protects scarce resources and sensitive credentials. The browser receives only short-lived tokens or signed URLs. Supabase performs atomic capacity decisions. LiveKit carries low-latency media and data channels. The API releases reservations on dispatch failure so capacity does not get stuck.

### When
It runs when `/v1/sessions`, `/v1/sessions/kfl`, `/v1/sessions/plugins/livekit`, `/v1/embed/create_session`, or preview-session endpoints are called. The browser half runs when `PersonaEmbed.connect()` or `PersonaSession.connect()` is invoked.

### How
The main path is:
1. Validate request shape: a session chooses exactly one `persona_id` or scoped `persona_slug`.
2. Resolve org limits and billing eligibility before reserving capacity.
3. Resolve persona visibility and plan-supported model.
4. Reserve a node by calling `find_candidate_nodes` then `reserve_capacity`.
5. Create a LiveKit room with short TTL access tokens for `user` and `agent` identities.
6. Dispatch the avatar worker to the selected node using `NODE_API_KEY`.
7. Browser connects to LiveKit, opens a byte stream on `lk.audio_stream`, subscribes to avatar tracks, and uses `lk.control` data packets for interrupts and emotion changes.
8. On error after reservation creation, API marks the reservation `failed_dispatch`; cron/reaper paths clean up stale rooms and expired reservations.

## Scope
| Layer | Logic To Learn | Evidence |
| --- | --- | --- |
| Admission | Billing, persona visibility, plan/model gates | modal_kflapi/app.py |
| Capacity | Healthy node selection, org concurrency, node capacity | supabase-kfl/supabase/migrations/20260109164203_nodes_and_reservations_rpcs.sql |
| Transport | LiveKit room, tokens, tracks, byte streams, control topic | js-sdk/src/PersonaSession.ts |
| Embed orchestration | Fetch session, init media, mic capture, provider adapter, cleanup | js-elements/src/PersonaEmbed.ts |
| Voice runtime | STT endpoint, LLM/TTS streaming, interruption | [[Workflows/KFL Voice Agent Session]] |
| Safety | Release-on-failure, stale reservation reaping, room limits | modal_kflapi/app.py, modal_kflapi_tests/test_session_lifecycle.py |

## Use Case
Use this note when debugging session startup, avatar silence, no nodes available errors, origin/auth failures, or interrupt behavior.

## Stage In Lifecycle
Runtime. It is the main live path of the product.

## Importance
Core. If this workflow fails, the product cannot deliver realtime personas.

## Risks
- 503 means no usable node capacity, not necessarily API outage.
- 429 means org-level concurrency is already consumed, and trying another node will not help.
- 402 means billing eligibility blocked the session before capacity reservation.
- Dispatch failure after reservation must release the reservation or the cluster appears artificially full.
- LiveKit topic names and participant identities are part of the protocol and must stay aligned with worker expectations.
- Browser audio autoplay and microphone permission failures can look like backend media failures.

## Input / Output
| Input | Output |
| --- | --- |
| API key or publishable key plus persona/config identity | LiveKit `server_url`, `participant_token`, and `agent_identity` |
| Browser microphone PCM | Voice-agent input stream and avatar audio/video output |
| Voice-agent audio PCM | LiveKit byte stream into avatar renderer |
| Interrupt/emotion events | Unreliable control data directed to the avatar agent |

## Resources
- [[Concepts/Reservation And Node Capacity]]
- [[Concepts/LiveKit Transport]]
- [[Concepts/Voice Agent Details]]
- [[Technologies/LiveKit]]
- [[Technologies/WebRTC]]
- [[Technologies/DataStreams]]
- [[Technologies/WebSockets]]
- LiveKit realtime media docs: https://docs.livekit.io/transport/media/
- LiveKit realtime data docs: https://docs.livekit.io/transport/data/
- W3C WebRTC specification: https://www.w3.org/TR/webrtc/

## Related Topics
- [[Workflows/KFL Voice Agent Session]]
- [[Workflows/Billing Usage And Session Limits]]
- [[Philosophies/Reservation-Based GPU Capacity]]
- [[Philosophies/Real-Time Streaming Pipeline]]
- [[Philosophies/Layered SDK Abstraction]]

## Evidence
- modal_kflapi/app.py
- js-sdk/src/PersonaSession.ts
- js-elements/src/PersonaEmbed.ts
- js-elements/src/agents/types.ts
- modal_kflapi_tests/test_session_lifecycle.py
- modal_kflapi_tests/test_embed_session.py
- supabase-kfl/supabase/migrations/20260109164203_nodes_and_reservations_rpcs.sql
- supabase-kfl/supabase/migrations/20260210120000_billing.sql
