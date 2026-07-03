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
# LiveKit Room

## Objective
The objective of a LiveKit room in this repo is to act as the realtime meeting place where the browser participant, avatar or agent participant, media tracks, byte streams, and control messages are scoped to one live session.

## Description
A LiveKit room is the runtime container that makes the session interactive. The backend creates or selects the room during [[Definitions/Backend Admission|backend admission]], mints a participant token, and returns connection details. The browser SDK then connects a `Room` instance with those credentials.

What it is:

- A realtime session container managed through LiveKit.
- The scope in which participants publish tracks and exchange data.
- The coordination object that lets [[Definitions/PersonaSession|PersonaSession]] receive avatar media and send audio/control traffic.

Why it exists:

- Avatar sessions need low-latency bidirectional media and data transport.
- The browser and worker need a shared runtime namespace without hard-coding direct peer addresses.
- LiveKit gives the system a proven room/participant/track abstraction instead of custom WebRTC signaling.

When it is used:

- When the central API admits a session and creates credentials.
- When `PersonaSession.connect()` calls `room.connect(serverUrl, participantToken)`.
- While remote tracks, byte streams, and data messages move between participants.

How it works in this repo:

1. Backend admission creates a room and token for the browser participant.
2. The API returns `server_url`, `participant_token`, and `agent_identity`.
3. The SDK constructs a LiveKit `Room` object.
4. The SDK connects to that room and listens for track subscription and disconnect events.
5. The SDK sends byte streams and control messages to the configured agent participant inside the room.

## Scope
| In Scope | Out Of Scope |
|---|---|
| Room identity and participant coordination | Avatar model weights |
| Browser connection through LiveKit SDK | Business billing decisions |
| Track, data, and byte-stream transport scope | Long-term session analytics |
| Session-specific realtime namespace | Non-realtime persistence |

## Use Case
Read this definition when you need to understand where browser media tracks, avatar tracks, audio byte streams, and control commands meet during a live session.

## Stage In Lifecycle
Runtime. The room is created during admission and used throughout the active session.

## Importance
Core. The LiveKit room is the transport boundary that binds frontend SDK behavior to backend worker behavior.

## Risks
- Room lifecycle drift can create sessions where the browser has credentials but the expected agent participant is unavailable.
- Participant identity mismatches can prevent tracks or streams from reaching the intended target.
- Room cleanup and worker shutdown must stay coordinated to avoid wasted capacity.

## Input / Output
| Direction | Value | Meaning |
|---|---|---|
| Input | room name or ID | Backend-created realtime namespace |
| Input | participant token | Credential allowing the browser to join |
| Input | server URL | LiveKit endpoint for the room |
| Output | remote tracks | Avatar audio/video media consumed by the browser |
| Output | data and byte-stream channels | Session-scoped commands and audio payloads |

## Resources
- [[Technologies/LiveKit]]
- [[Concepts/LiveKit Transport]]
- [[Definitions/PersonaSession|PersonaSession]]
- [[Definitions/Backend Admission|Backend Admission]]

## Related Topics
- [[Definitions/LiveKit Byte Stream|LiveKit Byte Stream]]
- [[Definitions/Control Message|Control Message]]
- [[Workflows/Real-Time Persona Session]]
- [[Architecture]]

## Evidence
- `js-sdk/src/PersonaSession.ts:111`
- `js-sdk/src/PersonaSession.ts:116`
- `js-sdk/src/PersonaSession.ts:121`
- `modal_kflapi/app.py`
