---
type: definition
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/definition
---
# PersonaSession

## Objective
The objective of `PersonaSession` is to manage one browser-side [[Definitions/Live Avatar Session|live avatar session]] by connecting to a [[Definitions/LiveKit Room|LiveKit room]], exposing remote avatar tracks, streaming generated PCM audio, sending realtime control commands, and cleaning up transport state.

## Description
`PersonaSession` is the central stateful object in [[Structure/js-sdk/js-sdk|js-sdk]]. It is created by [[Definitions/createClient|createClient()]] and then used by application code to connect, send audio, end audio turns, interrupt playback, set emotion, and close the session.

What it is:

- A browser-side class wrapping LiveKit's `Room` object.
- A lifecycle manager for connection state: disconnected, connecting, connected, and error.
- A transport adapter between application audio/control calls and LiveKit byte streams/data messages.

Why it exists:

- The app should not need to know every LiveKit primitive required by the avatar pipeline.
- The SDK needs a single place to coordinate room connection, track subscription, audio streaming, control messages, and cleanup.
- Realtime sessions have stateful ordering constraints; for example, byte-stream writes must not race with stream closure.

When it is used:

- Immediately after `createClient()` returns it.
- During a live avatar interaction, while the browser sends audio and receives avatar media.
- When the user interrupts, changes emotion, finishes an utterance, or leaves the session.

How it works:

1. `connect()` creates a LiveKit `Room`, installs listeners, and connects with `serverUrl` and `participantToken`.
2. Once connected, it opens a byte stream to `agentIdentity` on `lk.audio_stream`.
3. LiveKit track events are filtered so only the configured agent participant is surfaced to callbacks.
4. `sendAudio()` writes PCM bytes into the active byte stream through a queued write chain.
5. `endAudioTurn()` waits for pending writes, closes the byte-stream writer, and clears it.
6. `interrupt()` and `setEmotion()` publish low-latency data messages on `lk.control`.
7. `close()` closes the writer, disconnects the room, and returns the object to a disconnected state.

## Scope
| In Scope | Out Of Scope |
|---|---|
| Browser-side LiveKit connection management | Server-side room creation |
| Track subscription callbacks for avatar audio/video | Avatar model inference |
| PCM byte-stream writes | Speech recognition and generation logic |
| Control messages such as interrupt and emotion | Billing or capacity enforcement |
| Client-side cleanup | Long-term persistence |

## Use Case
Use `PersonaSession` when reading or building custom frontend session logic that needs direct control over connection, callbacks, audio writes, turn boundaries, interruptions, emotion commands, and teardown.

## Stage In Lifecycle
Runtime. It is active for the browser portion of the session lifecycle after backend admission and before final cleanup.

## Importance
Core. `PersonaSession` is where the JS SDK turns static credentials into realtime behavior. It is the best single file for understanding how the browser participates in the avatar session.

## Risks
- Audio writes must stay ordered; closing a byte stream before queued writes finish can truncate audio.
- Track callbacks must stay scoped to `agentIdentity`; otherwise unrelated participant tracks could leak into the UI.
- Control messages are sent as unreliable data, which is appropriate for low-latency commands but not for durable state.
- Error and close handling must avoid leaving stale room or writer references behind.

## Input / Output
| Direction | Value | Meaning |
|---|---|---|
| Input | `serverUrl` | LiveKit server URL used by `Room.connect()` |
| Input | `participantToken` | Token authorizing the browser participant |
| Input | `agentIdentity` | Participant identity used to scope streams, messages, and track callbacks |
| Input | PCM audio bytes | Generated or captured audio to send over `lk.audio_stream` |
| Input | emotion string | Emotion command sent over `lk.control` |
| Output | media track callbacks | Remote avatar audio/video tracks surfaced to app code |
| Output | state callbacks | Session and agent-state changes surfaced to app code |
| Output | LiveKit data/byte-stream traffic | Runtime transport messages to the agent/avatar participant |

## Resources
- [[Structure/js-sdk/js-sdk]]
- [[Definitions/createClient|createClient()]]
- [[Definitions/LiveKit Room|LiveKit Room]]
- [[Definitions/LiveKit Byte Stream|LiveKit Byte Stream]]
- [[Definitions/Control Message|Control Message]]

## Related Topics
- [[Concepts/LiveKit Transport]]
- [[Technologies/LiveKit]]
- [[Technologies/DataStreams]]
- [[Definitions/Backend Admission|Backend Admission]]
- [[Definitions/Live Avatar Session|Live Avatar Session]]

## Evidence
- `js-sdk/src/PersonaSession.ts:46`
- `js-sdk/src/PersonaSession.ts:49`
- `js-sdk/src/PersonaSession.ts:76`
- `js-sdk/src/PersonaSession.ts:111`
- `js-sdk/src/PersonaSession.ts:121`
- `js-sdk/src/PersonaSession.ts:194`
- `js-sdk/src/PersonaSession.ts:218`
- `js-sdk/src/PersonaSession.ts:258`
- `js-sdk/src/PersonaSession.ts:284`
- `js-sdk/src/PersonaSession.ts:305`
