---
type: definition
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: high
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/realtime
---

# LiveKit Agent

## Objective
Define a LiveKit agent as used around the Keyframe avatar integration and clarify its relationship to LiveKit rooms and WebRTC.

## Description
A LiveKit agent is server-side code that joins a LiveKit room as a participant and can listen to audio, call AI services, publish media, and send/receive data. LiveKit provides the WebRTC room and transport; an agent is one program running inside that transport.

## Scope
| Included | Excluded |
| --- | --- |
| Server-side participant/bot behavior | The LiveKit service itself |
| AI participant that may use STT, LLM, and TTS | Browser-only UI components |
| Plugin path where Keyframe avatar joins an existing LiveKit Agents room | Generic WebRTC without LiveKit room semantics |

## Use Case
Use this definition when distinguishing LiveKit as transport from the application code that joins the room and performs AI work.

## Stage In Lifecycle
Runtime.

## Importance
High. Misunderstanding this boundary makes it easy to confuse WebRTC connection setup with the AI voice/agent behavior running on top of it.

## Risks
- LiveKit can connect successfully while the agent logic still fails.
- Agent tokens, participant identities, and room permissions must match the avatar protocol.
- Server-side agent credentials should not be exposed to browser code.

## Input / Output
| Input | Output |
| --- | --- |
| LiveKit room/token plus agent code | AI participant that can publish/subscribe in the room |
| User audio/media/data | Agent-side STT/LLM/TTS or control behavior |

## Resources
- [[Concepts/LiveKit Transport]]
- [[Technologies/LiveKit]]
- [[Structure/livekit-plugins-keyframe/livekit-plugins-keyframe]]
- [[Concepts/Voice AI Pipeline]]

## Related Topics
- [[Definitions/LiveKit Room]]
- [[Technologies/WebRTC]]
- [[Philosophies/Real-Time Streaming Pipeline]]

## Evidence
- livekit-plugins-keyframe/README.md
- livekit-plugins-keyframe/livekit/plugins/keyframe/avatar.py
- modal_kflapi/app.py
- kfl-voice-agent/ARCHITECTURE.md
- js-sdk/src/PersonaSession.ts
