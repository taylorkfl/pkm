---
type: structure-note
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
repo_path: js-sdk
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/frontend
---

# js-sdk

## Objective
The objective of `js-sdk` is to provide the lowest-level public browser [[Definitions/SDK|SDK]] API for a Keyframe Labs [[Definitions/Live Avatar Session|live avatar session]]: take server-minted LiveKit session credentials from [[Definitions/Backend Admission|backend admission]], connect the browser to a [[Definitions/LiveKit Room|LiveKit room]], expose avatar audio/video tracks, stream generated PCM audio through a [[Definitions/LiveKit Byte Stream|LiveKit byte stream]], send low-latency [[Definitions/Control Message|control messages]] such as interrupt and emotion, and cleanly close the transport.

It intentionally does not own UI, microphone capture, voice-agent orchestration, billing, persona resolution, or session creation. Those responsibilities live in higher layers or backend APIs.

## Description
### What
`js-sdk` is the transport core underneath `@keyframelabs/elements` and `@keyframelabs/react`. Its main class is [[Definitions/PersonaSession|PersonaSession]], exported through [[Definitions/createClient|createClient()]]. The SDK assumes a backend has already called the Keyframe API and returned:

- `serverUrl`: the LiveKit/WebRTC server URL.
- `participantToken`: a short-lived room token for the browser participant.
- `agentIdentity`: the LiveKit identity of the avatar/agent participant.

With those inputs, the SDK joins the LiveKit room, subscribes only to tracks from the configured agent identity, forwards those tracks through callbacks, opens a LiveKit byte stream for generated audio, and publishes control messages to the same agent identity.

### Why
The SDK exists to give advanced integrators a thin, explicit control surface. Customers who bring their own agent, realtime LLM, microphone pipeline, or UI can use `js-sdk` without adopting the framework-agnostic element layer or the React wrapper. This keeps the transport contract stable while allowing higher layers to add opinionated UI and provider bindings.

The design also keeps trust boundaries clean. The SDK never receives the customer’s secret API key. It only receives session details that were already minted server-side by the central API after auth, billing, persona visibility, model-plan checks, and GPU reservation.

### When
Use `js-sdk` when the consumer wants high control:

- A custom app already has its own UI and state management.
- A custom voice agent or realtime LLM will produce PCM audio.
- The app wants to handle media elements directly through callbacks.
- The app needs explicit control over `sendAudio`, `endAudioTurn`, `interrupt`, `setEmotion`, `flush`, and `close`.

Higher-level apps should use `js-elements` or `js-react` when they want microphone capture, provider adapter wiring, DOM media setup, and status state handled for them.

### How
The main runtime sequence is:

1. A trusted server calls the central API and returns LiveKit session details to the browser.
2. The browser calls [[Definitions/createClient|createClient(options)]], which returns a [[Definitions/PersonaSession|PersonaSession]].
3. `connect()` guards against duplicate connection attempts, sets state to `connecting`, creates a LiveKit `Room`, registers listeners/RPC handlers, and calls `room.connect(serverUrl, participantToken)`.
4. After room connection, the SDK opens a byte stream on topic `lk.audio_stream` directed to `agentIdentity`.
5. When the agent publishes audio/video tracks, `TrackSubscribed` filters by `participant.identity === agentIdentity` before firing `onVideoTrack` or `onAudioTrack`.
6. `sendAudio(pcmData)` queues writes to the byte stream so audio chunks stay ordered and failures do not break the caller synchronously.
7. `endAudioTurn()` waits behind pending writes, closes the current byte stream, and clears the writer so the next turn can open a fresh stream.
8. `interrupt()` publishes unreliable `lk.control` data containing `interrupt`, directed to the agent.
9. `setEmotion(emotion)` publishes unreliable `lk.control` data containing `emotion:<label>`, directed to the agent.
10. `close()` waits for pending byte-stream cleanup, disconnects the room, clears local references, and reports `disconnected`.

## Scope
| File | Responsibility | What To Learn |
| --- | --- | --- |
| `js-sdk/src/index.ts` | Public package surface | [[Definitions/createClient|createClient()]] is intentionally a thin factory around [[Definitions/PersonaSession|PersonaSession]]; exported types/utilities define the supported public API. |
| `js-sdk/src/PersonaSession.ts` | Runtime session implementation | LiveKit connection, track filtering, byte stream audio, RPC playback state, interruption, emotion, write queue, and cleanup. |
| `js-sdk/src/types.ts` | Public contract types | Session state, close reasons, agent playback state, emotion labels, and required/optional options. |
| `js-sdk/src/audio-utils.ts` | Shared low-level audio helpers | PCM constants, base64 conversion, simple linear resampling, and typed event-emitter utility. |
| `js-sdk/README.md` | Consumer-facing model | Explains why this package is the high-control option compared with elements and React. |

### In Scope
- LiveKit room connection from already-created session credentials.
- Track subscription callbacks for avatar media.
- PCM byte-stream delivery to the avatar/agent participant.
- Lightweight control signaling for interrupt and emotion.
- Session state and close/error callbacks.
- Audio utility exports useful to SDK consumers and higher layers.

### Out Of Scope
- Creating sessions with secret API keys.
- Resolving personas, billing, plan limits, or GPU reservations.
- Capturing microphone audio.
- Connecting to [[Technologies/OpenAI Realtime]], [[Technologies/ElevenLabs]], [[Technologies/Cartesia]], [[Technologies/Soniox]], [[Technologies/OpenRouter]], or KFL voice agent providers.
- Rendering UI controls, overlays, status indicators, or hosted widgets.

## Use Case
Use this note when:

- You need to understand the lowest-level browser contract for realtime Persona sessions.
- You are debugging avatar tracks arriving, generated audio not reaching the avatar, interruption not clearing output, or emotion control not changing expression.
- You are deciding whether a feature belongs in `js-sdk`, `js-elements`, `js-react`, or the backend.
- You are checking whether a protocol change must be mirrored in LiveKit worker/runtime code.

As a rule of thumb: if the change requires microphone capture, provider-specific WebSocket logic, DOM media management, or UI state, it probably belongs above `js-sdk`. If the change is about LiveKit session transport, byte stream topics, participant identity, callbacks, or control messages, it may belong here.

## Stage In Lifecycle
Runtime. `js-sdk` runs after backend admission has completed and before/during the live avatar session. It is not part of discovery, billing, persistence, or deployment; it is the browser-side transport layer used during an active session.

## Importance
Core. `js-sdk` is the foundation under the higher-level embed packages. If this layer’s protocol drifts from the backend or avatar worker, every consumer above it can appear broken even when UI and provider logic are correct.

Its importance comes from four invariants:

| Invariant | Why It Matters |
| --- | --- |
| `agentIdentity` must match the avatar participant | Track filtering, byte stream destination, and control messages all depend on it. |
| `lk.audio_stream` must match the worker expectation | Generated agent audio reaches the avatar through this byte stream topic. |
| `lk.control` must match the worker expectation | Interrupt and emotion messages are routed through this topic. |
| Audio turns must be ordered and explicitly ended | The write queue and byte-stream close behavior preserve turn boundaries. |

## Risks
- **Protocol drift:** Topic names, RPC method names, participant identities, or emotion labels can drift from the avatar worker/runtime.
- **Wrong layer changes:** Adding provider-specific or UI-specific behavior here would make the low-level SDK less reusable.
- **Silent media mismatch:** If `agentIdentity` is wrong, LiveKit can connect successfully while media callbacks never fire because tracks are filtered out.
- **Dropped controls:** `interrupt()` and `setEmotion()` use unreliable data publishing for low latency, so downstream systems must tolerate missed control packets.
- **Audio ordering bugs:** Direct concurrent writes could reorder chunks; the write queue is part of the transport contract.
- **Turn-boundary bugs:** Failing to call `endAudioTurn()` can leave the current byte stream open and confuse downstream turn processing.
- **Documentation drift:** The README uses `/v1/session` while the central API evidence uses `/v1/sessions`; verify before copying docs into integration guidance.

## Input / Output
| Input | Source | Output | Consumer |
| --- | --- | --- | --- |
| `serverUrl` | Central API session response | LiveKit/WebRTC connection | Browser room |
| `participantToken` | Central API session response | Authenticated browser participant | LiveKit server |
| `agentIdentity` | Central API session response | Track filter and data destination | Avatar/agent participant |
| Remote audio/video tracks | LiveKit `TrackSubscribed` event | `onAudioTrack` / `onVideoTrack` callbacks | Consumer app or higher embed layer |
| 24 kHz signed 16-bit PCM bytes | Consumer voice agent or higher layer | Byte-stream writes on `lk.audio_stream` | Avatar worker |
| Agent playback RPCs | Avatar worker | `onAgentStateChange('speaking' | 'listening')` | Consumer UI/state |
| `interrupt()` call | Consumer app or higher layer | Unreliable `lk.control` message `interrupt` | Avatar worker |
| `setEmotion(emotion)` call | Consumer app or higher layer | Unreliable `lk.control` message `emotion:<label>` | Avatar worker |
| `close()` call or LiveKit disconnect | Consumer/server/runtime | Closed room, cleared writer, close/state callback | Consumer app |

## Resources
### Key Definitions
- [[Definitions/Definition Index]]
- [[Definitions/SDK|SDK]]
- [[Definitions/createClient|createClient()]]
- [[Definitions/PersonaSession|PersonaSession]]
- [[Definitions/Backend Admission|Backend Admission]]
- [[Definitions/Live Avatar Session|Live Avatar Session]]
- [[Definitions/LiveKit Room|LiveKit Room]]
- [[Definitions/LiveKit Byte Stream|LiveKit Byte Stream]]
- [[Definitions/Control Message|Control Message]]

### Related Notes
- [[Workflows/Real-Time Persona Session]]
- [[Concepts/LiveKit Transport]]
- [[Technologies/LiveKit]]
- [[Technologies/WebRTC]]
- [[Technologies/DataStreams]]
- [[Structure/js-elements/js-elements]]
- [[Structure/js-react/js-react]]
- LiveKit realtime media docs: https://docs.livekit.io/transport/media/
- LiveKit realtime data docs: https://docs.livekit.io/transport/data/
- W3C WebRTC specification: https://www.w3.org/TR/webrtc/

## Related Topics
- [[Architecture]]
- [[Philosophies/Layered SDK Abstraction]]
- [[Philosophies/Real-Time Streaming Pipeline]]
- [[Concepts/Voice Agent Details]]
- [[Concepts/Emotion Conditioning]]
- [[Workflows/Test-Driven Reading Path]]

## Evidence
- js-sdk/src/index.ts
- js-sdk/src/PersonaSession.ts
- js-sdk/src/types.ts
- js-sdk/src/audio-utils.ts
- js-sdk/README.md
- js-elements/src/PersonaEmbed.ts
- modal_kflapi/app.py
