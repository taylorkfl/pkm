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

# LiveKit Transport

## Objective
Explain how LiveKit is used as the media and data transport between browser participants and the avatar worker.

## Description
### What
LiveKit is the realtime room layer for avatar sessions. In Keyframe-hosted sessions, the central API creates a room, mints role-scoped tokens, the browser joins as `user`, the avatar worker joins as `agent`, avatar media arrives as remote audio/video tracks, and browser-produced agent audio/control messages travel through LiveKit data/byte channels. In plugin sessions, a LiveKit Agents app supplies the room and token, and Keyframe dispatches the avatar worker into that existing room.

### Why
WebRTC gives low-latency media transport, NAT traversal, adaptive networking, and track subscription semantics. LiveKit packages those primitives into rooms, participants, access tokens, tracks, RPC, and data streams so the SDK can avoid hand-building WebRTC signaling.

### When
It runs after the central API admits a session and before the browser can display avatar media. It continues for the session lifetime.

### How
`PersonaSession.connect()` creates a LiveKit `Room`, subscribes to tracks from the configured agent identity, opens a byte stream to the agent on `lk.audio_stream`, registers RPC handlers for playback state signals, and publishes unreliable data messages on `lk.control` for interruption and emotion.

## Scope
| LiveKit Piece | Repo Meaning |
| --- | --- |
| Room | One realtime avatar session scope |
| User token | Browser participant may join, subscribe, and publish data |
| Agent token | Worker/avatar participant may publish and subscribe |
| Plugin room/token | External LiveKit Agents app asks Keyframe to attach an avatar worker |
| Remote video/audio tracks | Avatar media displayed in the embed |
| Byte stream topic `lk.audio_stream` | PCM agent audio delivered to avatar worker |
| Data topic `lk.control` | Interrupt/emotion control messages |
| RPC playback methods | Agent tells SDK when playback starts/finishes |

## Use Case
Use when debugging media connection, avatar tracks, audio stream delivery, interrupts, or SDK/worker protocol alignment.

## Stage In Lifecycle
Runtime. It is active for every LiveKit-backed persona session.

## Importance
Core. It is the transport layer between browser and avatar runtime.

## Risks
- Topic names and participant identities are protocol, not incidental strings.
- Short token TTLs reduce exposure but can complicate slow client startup.
- Unreliable control data is appropriate for low-latency control but can drop under network pressure.
- Hosted SDK sessions and LiveKit Agents plugin sessions must agree on the same transport protocol.
- Browser autoplay can make successful audio tracks appear silent.
- WebRTC complexity is hidden by LiveKit but still surfaces as track/subscription/network failures.

## Input / Output
| Input | Output |
| --- | --- |
| LiveKit server URL and participant token | Connected browser room participant |
| Agent-published tracks | Browser media elements receive audio/video |
| Browser PCM/control data | Avatar worker receives stream/control events |

## Resources
- [[Definitions/LiveKit Agent]]
- [[Concepts/Voice AI Pipeline]]
- [[Technologies/LiveKit]]
- [[Technologies/LiveKit]]
- [[Concepts/RPC Over WebRTC]]
- [[Technologies/WebRTC]]
- [[Technologies/DataStreams]]
- [[Workflows/Real-Time Persona Session]]
- LiveKit realtime media docs: https://docs.livekit.io/transport/media/
- LiveKit realtime data docs: https://docs.livekit.io/transport/data/
- LiveKit Keyframe avatar plugin docs: https://docs.livekit.io/agents/models/avatar/plugins/keyframe/
- W3C WebRTC specification: https://www.w3.org/TR/webrtc/

## Related Topics
- [[Philosophies/Real-Time Streaming Pipeline]]
- [[Structure/js-sdk/js-sdk]]
- [[Structure/js-elements/js-elements]]
- [[Structure/livekit-plugins-keyframe/livekit-plugins-keyframe]]

## Evidence
- modal_kflapi/app.py
- js-sdk/src/PersonaSession.ts
- js-elements/src/PersonaEmbed.ts
- js-elements/src/agents/types.ts
- ml/windy-inference/session.py
- livekit-plugins-keyframe/livekit/plugins/keyframe/avatar.py
- livekit-plugins-keyframe/livekit/plugins/keyframe/api.py
