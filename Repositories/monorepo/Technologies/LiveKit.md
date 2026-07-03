---
type: technology
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/technology
  - pkm/domain/runtime
---

# LiveKit

## Objective
Explain what LiveKit is, how it supports Keyframe Labs product sessions and deployments, where it appears in the monorepo, and what a demo-app builder must keep straight.

## Description
### What
LiveKit is the realtime room, participant, media-track, data-stream, data-packet, RPC, and token layer used around WebRTC. In this repo it is infrastructure and transport: it is not the voice model, not the avatar model, and not the business API. It is the realtime meeting place where browsers, voice agents, and avatar workers exchange media and control messages.

### Why
It saves the product from hand-rolling WebRTC signaling, participant identity, short-lived room access, media track subscription, binary audio streaming, and low-latency control messages. This lets Keyframe focus product code on admission, persona selection, GPU reservation, avatar synthesis, SDK ergonomics, and demo surfaces.

### When
LiveKit is active during every realtime avatar session. It begins after backend admission creates or accepts a room and token, and it continues until the browser, LiveKit Agent, or avatar worker disconnects.

### How
There are two main integration paths:

1. Keyframe-hosted sessions: the central API validates the request, reserves GPU capacity, creates a LiveKit room, mints short-lived `user` and `agent` tokens, dispatches a GPU node with the LiveKit URL/token/room, and returns browser credentials to the SDK.
2. LiveKit Agents plugin sessions: a customer's LiveKit Agents app owns the room and agent session. The `livekit-plugins-keyframe` package mints or receives avatar-room credentials, calls Keyframe's `/v1/sessions/plugins/livekit` endpoint, and Keyframe dispatches an avatar worker into that existing room.

Read the repo files listed in Evidence first, then use the external source links to understand the underlying LiveKit behavior. Treat external docs as support for protocol/framework behavior and repo evidence as the source of truth for local implementation.

## Scope
| LiveKit Surface | Keyframe Meaning |
| --- | --- |
| Room | One realtime avatar-session scope |
| Participant identity | Browser, voice agent, or avatar worker routing key |
| Access token | Short-lived authorization for joining and publishing/subscribing |
| Remote audio/video tracks | Avatar media rendered by the browser/embed |
| Byte stream topic `lk.audio_stream` | PCM audio delivered to the avatar worker |
| Data topic `lk.control` | Interrupt and emotion control messages |
| RPC playback methods | Playback state signals between avatar worker and SDK |
| LiveKit Agents plugin | Customer-facing way to add a Keyframe avatar to a LiveKit agent |

## Use Case
Use when building a demo app, debugging session startup, diagnosing missing avatar media, editing SDK transport code, changing worker protocol, configuring deployment secrets, or explaining the `livekit-plugins-keyframe` package.

## Product And Deployment Role
### Keyframe-Hosted Sessions
The product backend creates the LiveKit room and token pair. `modal_kflapi/app.py` reads `LIVEKIT_URL`, `LIVEKIT_API_KEY`, and `LIVEKIT_API_SECRET`, creates a room with a small participant limit, mints a browser token for `user`, mints an avatar/agent token for `agent`, dispatches a GPU node, and returns `server_url`, `participant_token`, and `agent_identity`.

The browser then uses `js-sdk/src/PersonaSession.ts` to connect to the room. `js-elements` wires voice-agent audio into `PersonaSession.sendAudio()`, and the SDK sends that audio over the `lk.audio_stream` byte stream while using `lk.control` for interrupt and emotion messages.

The GPU worker uses `ml/windy-inference/session.py` to join the same LiveKit room, receive audio bytes, run avatar inference, and publish synchronized audio/video tracks back to the browser. The worker daemon receives `livekit_url`, `livekit_token`, `room_name`, and `source_participant_identity` during dispatch.

### LiveKit Agents Plugin Sessions
The plugin path is for builders already using LiveKit Agents. `livekit-plugins-keyframe` exposes `keyframe.AvatarSession`, which can be attached to a LiveKit `AgentSession`. The plugin calls Keyframe's plugin session endpoint with the caller's `room_name`, `livekit_url`, `livekit_token`, and source participant identity. Keyframe reserves GPU capacity and attaches an avatar worker to that room.

For demos, this means:
- Use Keyframe JS Elements/SDK when the demo app should let Keyframe own room creation and session admission.
- Use `livekit-plugins-keyframe` when the demo app is already a LiveKit Agents app and needs a Keyframe avatar participant inside its existing room.
- Never expose `LIVEKIT_API_SECRET` or privileged LiveKit server credentials to browser code.
- Keep `lk.audio_stream`, `lk.control`, playback RPC names, participant identities, and sample-rate expectations aligned across SDK, plugin, API, and worker code.

## Monorepo Usage Map
| Area | How LiveKit Is Used |
| --- | --- |
| `modal_kflapi/app.py` | Creates hosted session rooms/tokens and exposes `/v1/sessions/plugins/livekit` for plugin-launched avatar workers |
| `modal_common/main.py` | Centralizes LiveKit API client construction from deployment environment variables |
| `js-sdk/src/PersonaSession.ts` | Wraps `livekit-client` `Room`, subscribes to avatar tracks, streams bytes, sends control data, and handles playback RPCs |
| `js-elements/src/PersonaEmbed.ts` | Bridges voice-agent audio/events into the lower-level `PersonaSession` transport |
| `ml/windy-inference/session.py` | Joins rooms as the avatar worker, receives audio streams/control, and publishes avatar audio/video tracks |
| `ml/windy-inference/server/daemon.py` | Attaches reserved GPU slots to LiveKit rooms during dispatch |
| `livekit-plugins-keyframe` | Public LiveKit Agents plugin for starting Keyframe avatar sessions from LiveKit Agent code |
| `modal_kflapi_tests/test_plugin_session.py` | Guards plugin endpoint request validation, auth, persona resolution, and billing/model rules |
| `supabase-kfl/supabase/migrations/20260109163200_reservations.sql` | Defines reservation states that describe LiveKit join lifecycle such as `ready` and `in_use` |
| `pyproject.toml` | Declares LiveKit API, agents, plugin, and noise-cancellation dependencies |

## Stage In Lifecycle
Runtime and deployment. LiveKit is configured at deployment time through environment variables and used at runtime for every realtime avatar session.

## Importance
Core. This technology materially affects repo behavior in its domain.

## Risks
- Participant identities, topic names, and RPC method names are protocol, not incidental strings.
- Short token TTLs reduce credential exposure but can make slow startup paths fail in confusing ways.
- Unreliable control messages are fast but can be dropped under network pressure.
- Browser autoplay or track filtering can make a successful LiveKit connection look silent.
- Plugin, API, SDK, and worker code must stay aligned on avatar participant identity.
- External LiveKit docs explain the platform, but repo evidence defines Keyframe's concrete behavior.

## Input / Output
| Input | Output |
| --- | --- |
| LiveKit server URL and participant token | Browser or worker joins a room |
| Browser or agent PCM audio | Avatar worker receives byte-stream audio |
| Avatar inference output | Browser receives synchronized LiveKit audio/video tracks |
| Interrupt or emotion command | Avatar worker receives `lk.control` data |
| LiveKit Agents room details | Keyframe plugin session endpoint dispatches avatar worker |

## Resources
- LiveKit overview: https://docs.livekit.io/intro/overview/
- LiveKit realtime media docs: https://docs.livekit.io/transport/media/
- LiveKit realtime data docs: https://docs.livekit.io/transport/data/
- LiveKit RPC docs: https://docs.livekit.io/transport/data/rpc/
- LiveKit Keyframe avatar plugin docs: https://docs.livekit.io/agents/models/avatar/plugins/keyframe/

## Related Topics
- [[Technologies/Technology Index]]
- [[Architecture]]
- [[Concepts/Research Source Index]]
- [[Concepts/LiveKit Transport]]
- [[Concepts/RPC Over WebRTC]]
- [[Technologies/WebRTC]]
- [[Technologies/DataStreams]]
- [[Workflows/Real-Time Persona Session]]
- [[Structure/js-sdk/js-sdk]]
- [[Structure/js-elements/js-elements]]
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Structure/livekit-plugins-keyframe/livekit-plugins-keyframe]]

## Evidence
- js-sdk/src/PersonaSession.ts
- js-elements/src/PersonaEmbed.ts
- js-elements/src/PersonaView.ts
- modal_kflapi/app.py
- modal_common/main.py
- ml/windy-inference/session.py
- ml/windy-inference/server/daemon.py
- ml/windy-inference/ARCHITECTURE.md
- livekit-plugins-keyframe/README.md
- livekit-plugins-keyframe/livekit/plugins/keyframe/avatar.py
- livekit-plugins-keyframe/livekit/plugins/keyframe/api.py
- modal_kflapi_tests/test_plugin_session.py
- supabase-kfl/supabase/migrations/20260109163200_reservations.sql
- pyproject.toml
