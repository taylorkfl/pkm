---
type: concept
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/runtime
---

# Voice Agent Details

## Objective
Define the voice-agent credential/config object that lets browser adapters connect to OpenAI, ElevenLabs, or KFL without exposing raw server secrets.

## Description
### What
`voice_agent_details` is the session response payload that tells the browser which voice-agent adapter to use and what short-lived credential or signed URL to connect with.

### Why
The embed layer supports multiple voice backends while keeping the Persona/LiveKit path stable. A normalized details object lets `PersonaEmbed` create an adapter based on `type` and then wire all adapters into the same event vocabulary.

### When
It is returned from embed, preview, and KFL session endpoints. `PersonaEmbed.connect()` consumes it after fetching the session and before microphone capture starts streaming audio.

### How
Server-side code discriminates voice-agent config by `type`. For OpenAI, it mints a Realtime client secret; for ElevenLabs, it asks for a signed conversation URL; for KFL, it creates an HMAC-signed WebSocket URL tied to reservation/config claims. Browser code chooses `OpenAIRealtimeAgent`, `ElevenLabsAgent`, or `KflAgent` and forwards normalized events into `PersonaSession`.

## Scope
| Type | Browser Credential | Adapter Responsibility |
| --- | --- | --- |
| `openai` | Ephemeral token/client secret | Realtime WebSocket session, audio deltas, emotion tool calls |
| `elevenlabs` | Signed URL plus agent id | Conversational AI WebSocket, audio alignment, virtual turn-end timing |
| `kfl` | Signed KFL WebSocket URL | First-party STT/LLM/TTS runtime protocol |

## Use Case
Use when adding a voice provider, changing session response shape, debugging provider auth, or tracing how agent audio reaches the avatar.

## Stage In Lifecycle
Runtime. It is the browser-visible result of secret handling and session creation.

## Importance
Core. It lets the embed remain provider-agnostic while protecting server-held secrets.

## Risks
- Adapter event mismatches break the generic embed pipeline even if provider auth succeeds.
- Provider token minting failures should return gateway-style errors without leaking secrets.
- KFL signed URLs must be minted after reservation id exists.
- Sample-rate conversion must match provider expectations and Persona audio stream requirements.

## Input / Output
| Input | Output |
| --- | --- |
| Stored voice-agent config and encrypted key | Browser-safe details payload |
| Details payload | Connected adapter that emits normalized audio/turn/emotion/transcript events |

## Resources
- [[Technologies/OpenAI Realtime]]
- [[Technologies/ElevenLabs]]
- [[Technologies/WebSockets]]
- [[Concepts/Publishable Key And Secret Key]]
- OpenAI Realtime docs: https://platform.openai.com/docs/guides/realtime
- ElevenAgents WebSocket docs: https://elevenlabs.io/docs/eleven-agents/libraries/web-sockets

## Related Topics
- [[Workflows/Real-Time Persona Session]]
- [[Workflows/KFL Voice Agent Session]]
- [[Philosophies/Layered SDK Abstraction]]

## Evidence
- modal_common/agents.py
- modal_kflapi/app.py
- js-elements/src/PersonaEmbed.ts
- js-elements/src/agents/types.ts
- js-elements/src/agents/openai-realtime.ts
- js-elements/src/agents/elevenlabs.ts
- js-elements/src/agents/kfl.ts
