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
# Control Message

## Objective
The objective of a control message in the JS SDK is to send low-latency commands, such as interrupt and emotion changes, from the browser to the configured agent/avatar participant without waiting for slower request/response paths.

## Description
`PersonaSession` uses the `lk.control` topic for realtime control commands. `interrupt()` publishes `interrupt`; `setEmotion(emotion)` publishes `emotion:<emotion>`. Both are targeted at `agentIdentity` and sent as unreliable data because they are immediate runtime signals rather than durable records.

What it is:

- A small LiveKit data message sent during a connected session.
- A command channel separate from audio byte streams and media tracks.
- The browser's way to steer live avatar behavior while the session is active.

Why it exists:

- Interruption and emotion changes should be fast.
- Not every runtime command deserves a backend API round trip.
- Control commands belong to the active room participant context.

When it is used:

- When a user interrupts playback or speech.
- When app logic sets the avatar emotion.
- During a connected LiveKit session where the agent participant can receive commands.

How it works:

1. The SDK verifies that the session has an active room and connected state.
2. It encodes a short text payload.
3. It publishes the payload on `lk.control` to `agentIdentity`.
4. The receiving participant interprets the command in the active runtime context.

## Scope
| In Scope | Out Of Scope |
|---|---|
| `interrupt` command | Durable command audit log |
| `emotion:<emotion>` command | Backend admission decisions |
| Low-latency LiveKit data messages | Audio byte payload delivery |
| Agent-targeted runtime control | UI component styling |

## Use Case
Use this definition when tracing how a frontend action changes live avatar behavior without creating a new session or calling a backend endpoint.

## Stage In Lifecycle
Runtime. Control messages happen only while the browser is connected to the LiveKit room.

## Importance
High. Control messages are a key part of making the avatar feel interactive instead of merely streaming media.

## Risks
- Unreliable delivery is appropriate for immediacy but not for commands that must be guaranteed.
- Payload strings are small implicit protocols; sender and receiver must stay aligned.
- Emotion names must stay consistent with the SDK `Emotion` type and receiving runtime behavior.

## Input / Output
| Direction | Value | Meaning |
|---|---|---|
| Input | `interrupt` | Command to stop current playback or response |
| Input | `emotion:<emotion>` | Command to change avatar emotional state |
| Input | `agentIdentity` | Destination participant identity |
| Output | `lk.control` data message | Runtime signal sent to the agent/avatar participant |

## Resources
- [[Definitions/PersonaSession|PersonaSession]]
- [[Concepts/Emotion Conditioning]]
- [[Technologies/DataStreams]]
- [[Concepts/LiveKit Transport]]

## Related Topics
- [[Definitions/LiveKit Byte Stream|LiveKit Byte Stream]]
- [[Definitions/Live Avatar Session|Live Avatar Session]]
- [[Structure/js-sdk/js-sdk]]

## Evidence
- `js-sdk/src/PersonaSession.ts:49`
- `js-sdk/src/PersonaSession.ts:258`
- `js-sdk/src/PersonaSession.ts:284`
- `js-sdk/src/types.ts:6`
