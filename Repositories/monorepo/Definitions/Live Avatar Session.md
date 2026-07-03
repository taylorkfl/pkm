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
# Live Avatar Session

## Objective
The objective of a live avatar session is to create a bounded realtime interaction where a user-facing browser client, backend admission flow, LiveKit room, and avatar/voice worker cooperate to produce interactive avatar media and respond to user control.

## Description
A live avatar session is the end-to-end runtime thing the product is trying to deliver. It is broader than [[Definitions/PersonaSession|PersonaSession]], which is only the browser-side SDK object, and broader than [[Definitions/LiveKit Room|LiveKit Room]], which is the realtime transport container.

What it is:

- A product-level interaction lifecycle.
- A coordination of frontend SDK, central API, LiveKit, worker/runtime services, and session policy.
- The practical unit a user experiences as “talking to the avatar.”

Why it exists:

- The repository's main runtime value is not a single API call or package. It is a coordinated realtime experience.
- The session boundary gives product, billing, transport, inference, and UI code a shared lifecycle to reason about.
- It lets notes distinguish implementation pieces from the end-user runtime outcome.

When it is used:

- When a user starts an avatar/persona interaction.
- When backend admission creates the room and dispatches capacity.
- While the frontend sends audio/control and receives avatar tracks.
- Until the user, backend, or room lifecycle closes the session.

How it works:

1. Product UI requests a session.
2. Backend admission validates the request and creates realtime credentials.
3. The browser SDK creates a `PersonaSession` and joins the LiveKit room.
4. Runtime workers join or are dispatched for the target persona/avatar.
5. The browser and worker exchange media, byte-stream audio, and control messages.
6. The session ends through explicit close, room deletion, participant removal, or runtime shutdown.

## Scope
| In Scope | Out Of Scope |
|---|---|
| Full user-facing realtime session lifecycle | Offline research dataset analysis |
| Browser, API, LiveKit, and worker coordination | Static marketing pages |
| Media/control transport during an active session | Training a base model from scratch |
| Session admission and teardown semantics | General database administration |

## Use Case
Use this definition when a note needs to refer to the whole runtime interaction without collapsing it into one implementation file or package.

## Stage In Lifecycle
Runtime. It is the main product lifecycle that ties frontend, backend, transport, and worker code together.

## Importance
Core. This is the repo's central runtime workflow; many technologies and philosophies exist to make this session reliable, low-latency, and embeddable.

## Risks
- Session notes can become confusing if they do not distinguish browser session objects, backend session rows, LiveKit rooms, and user-visible sessions.
- Latency-sensitive pieces can fail independently; the user experiences them as one broken session.
- Cleanup needs to be coordinated across frontend, backend, LiveKit, and worker processes.

## Input / Output
| Direction | Value | Meaning |
|---|---|---|
| Input | user intent and persona selection | The product request to start interaction |
| Input | backend admission decision | Permission and capacity to run the session |
| Input | browser audio/control | User-side realtime interaction data |
| Output | avatar audio/video tracks | Media experienced by the user |
| Output | session state and close reason | Lifecycle feedback to product UI and backend systems |

## Resources
- [[Workflows/Real-Time Persona Session]]
- [[Structure/js-sdk/js-sdk]]
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Definitions/Backend Admission|Backend Admission]]

## Related Topics
- [[Definitions/PersonaSession|PersonaSession]]
- [[Definitions/LiveKit Room|LiveKit Room]]
- [[Definitions/LiveKit Byte Stream|LiveKit Byte Stream]]
- [[Definitions/Control Message|Control Message]]
- [[Philosophies/Real-Time Streaming Pipeline]]

## Evidence
- `js-sdk/src/PersonaSession.ts`
- `modal_kflapi/app.py`
- `kfl-voice-agent`
- `livekit-plugins-keyframe`
