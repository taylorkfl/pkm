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

# RPC Over WebRTC

## Objective
Explain the LiveKit RPC layer used for session control signals between the browser SDK and avatar worker.

## Description
### What
RPC over WebRTC is LiveKit's request/response method layer inside a room. In this repo it is used for playback-state and buffer-control signals that need a named method call between room participants.

### Why
Not every runtime signal is media. Some messages need to tell the other participant that avatar playback started, playback finished, or buffered audio should be cleared. LiveKit RPC lets the SDK and worker call named methods without adding a separate HTTP path.

### When
It runs during a connected LiveKit avatar session after both participants have joined the room.

### How
The browser SDK registers RPC methods for playback events. The worker performs RPC calls to the browser participant when playback starts and finishes. The worker also registers a local clear-buffer RPC so the SDK or upstream session logic can request a buffer clear. These names are protocol strings shared across TypeScript and Python.

## Scope
| RPC Method | Direction | Purpose |
| --- | --- | --- |
| `lk.playback_started` | Worker to SDK | Tell the browser/avatar session that generated playback has begun |
| `lk.playback_finished` | Worker to SDK | Tell the browser/avatar session that generated playback has drained |
| `lk.clear_buffer` | SDK/control side to worker | Ask the worker to clear queued audio/avatar data |

## Use Case
Use this note when debugging talk-over behavior, playback state, interruption behavior, or SDK/worker protocol mismatches.

## Stage In Lifecycle
Runtime. It is active only inside a connected LiveKit room.

## Importance
Core for reliable realtime feel. These calls help UI and voice-agent logic know when speech has actually started or finished.

## Risks
- Do not confuse LiveKit RPC methods with Supabase/Postgres RPC functions such as `reserve_capacity`.
- Method names are shared protocol. Rename them only across all participants at once.
- RPC assumes the target participant is connected and has registered the method.
- Playback text can arrive before audio is done. Use playback-finished signals when sequencing multi-agent conversation turns.
- RPC is useful for discrete method calls; byte streams and data packets still carry audio/control streams.

## Input / Output
| Input | Output |
| --- | --- |
| LiveKit room participants with registered methods | Named request/response calls over the room |
| Worker playback lifecycle | SDK state updates for speaking/listening |
| Clear-buffer request | Worker clears pending playback or inference buffer |

## Resources
- [[Technologies/LiveKit]]
- [[Concepts/LiveKit Transport]]
- [[Technologies/WebRTC]]
- [[Workflows/Real-Time Persona Session]]
- LiveKit RPC docs: https://docs.livekit.io/transport/data/rpc/

## Related Topics
- [[Structure/js-sdk/js-sdk]]
- [[Structure/livekit-plugins-keyframe/livekit-plugins-keyframe]]

## Evidence
- js-sdk/src/PersonaSession.ts
- ml/windy-inference/session.py
- ml/windy-inference/ARCHITECTURE.md
