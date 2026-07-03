---
type: definition
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: high
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/definition
---
# LiveKit Byte Stream

## Objective
The objective of the LiveKit byte stream in the JS SDK is to carry ordered PCM audio bytes from the browser session object to the configured agent/avatar participant during a live avatar session.

## Description
In [[Definitions/PersonaSession|PersonaSession]], the byte stream is opened with topic `lk.audio_stream` and targeted at `agentIdentity`. The SDK writes PCM data into that stream through `sendAudio()` and closes it at a turn boundary through `endAudioTurn()`.

What it is:

- A LiveKit data-stream mechanism for raw byte payloads.
- The SDK's audio payload path toward the avatar/agent participant.
- A lower-level transport primitive hidden behind the SDK API.

Why it exists:

- Audio payloads need ordered delivery semantics separate from UI callbacks.
- The SDK needs a way to send generated or captured PCM audio without pretending it is a normal media track.
- Turn boundaries need explicit closure behavior so the receiving participant knows when the current audio stream is complete.

When it is used:

- After `PersonaSession.connect()` successfully connects to the room.
- Whenever app code calls `sendAudio(pcmData)`.
- When app code calls `endAudioTurn()` to close the current stream.

How it works:

1. `connect()` opens a byte stream on `lk.audio_stream` to `agentIdentity`.
2. `sendAudio()` checks that the session is connected and a writer exists.
3. Writes are chained through `writeQueue` to preserve ordering.
4. `endAudioTurn()` waits for queued writes, closes the writer, and clears it.

## Scope
| In Scope | Out Of Scope |
|---|---|
| PCM byte payload delivery | Server-side speech generation internals |
| Byte-stream topic `lk.audio_stream` | Persistent audio storage |
| Write ordering and stream closure | UI rendering |
| Agent-targeted audio transport | Billing/session-limit policy |

## Use Case
Use this definition when tracing why audio is sent with `streamBytes()` instead of through a conventional callback or REST upload.

## Stage In Lifecycle
Runtime. It is active during the connected part of a live avatar session.

## Importance
High. Audio transport quality directly affects the user's perception of latency and reliability.

## Risks
- Missing writer checks can make audio sends fail silently or throw after disconnect.
- Closing the stream before queued writes finish can truncate audio.
- Topic or destination identity drift can send audio to the wrong runtime listener.

## Input / Output
| Direction | Value | Meaning |
|---|---|---|
| Input | `Int16Array`, `Uint8Array`, or `ArrayBuffer` PCM data | Audio payload provided by app code |
| Input | `agentIdentity` | Destination participant identity |
| Output | `lk.audio_stream` byte stream writes | Ordered audio bytes received by the target participant |

## Resources
- [[Definitions/PersonaSession|PersonaSession]]
- [[Definitions/LiveKit Room|LiveKit Room]]
- [[Technologies/DataStreams]]
- [[Concepts/LiveKit Transport]]

## Related Topics
- [[Definitions/Control Message|Control Message]]
- [[Philosophies/Real-Time Streaming Pipeline]]
- [[Structure/js-sdk/js-sdk]]

## Evidence
- `js-sdk/src/PersonaSession.ts:46`
- `js-sdk/src/PersonaSession.ts:121`
- `js-sdk/src/PersonaSession.ts:194`
- `js-sdk/src/PersonaSession.ts:218`
