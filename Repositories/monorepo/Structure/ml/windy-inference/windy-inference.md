---
type: structure-note
repo: monorepo
repo_path: ml/windy-inference
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/ml
---

# windy-inference

## Objective
Explain the real-time avatar video inference subsystem.

## Description
`ml/windy-inference` is the GPU-side avatar video synthesis runtime. It receives audio through LiveKit/session infrastructure, coordinates a daemon and worker, runs batched inference, and publishes synchronized avatar video.

## Scope
| Path | Role |
| --- | --- |
| `ml/windy-inference/ARCHITECTURE.md` | High-level architecture and flow |
| `ml/windy-inference/server/bridge_api.py` | HTTP bridge API on GPU node |
| `ml/windy-inference/server/daemon.py` | Session/worker management daemon |
| `ml/windy-inference/session.py` | Avatar session runtime |
| `ml/windy-inference/inference/worker.py` | Batched GPU inference worker |
| `ml/windy-inference/inference/shared_memory.py` | Shared-memory slot pool protocol |

## Use Case
Use this note when tracing how audio becomes avatar video or when changing GPU node runtime behavior.

## Stage In Lifecycle
Runtime and ML operations.

## Importance
Core. This subsystem is the avatar media generation backend.

## Risks
- Requires GPU/runtime environment assumptions not provided by normal frontend local dev.
- Shared-memory IPC and batching need careful concurrency handling.
- Session cleanup must stay synchronized with Supabase/node state.

## Input / Output
| Input | Output |
| --- | --- |
| Buffered audio/session request | Avatar video frames and media output |

## Resources
- [[Technologies/GPU Inference]]
- [[Technologies/PyTorch]]
- [[Concepts/LiveKit Transport]]
- [[Workflows/Real-Time Persona Session]]

## Related Topics
- [[Structure/kfl-voice-agent/kfl-voice-agent]]
- [[Concepts/Voice AI Pipeline]]
- [[Concepts/Reservation And Node Capacity]]

## Evidence
- ml/windy-inference/ARCHITECTURE.md
- ml/windy-inference/session.py
- ml/windy-inference/server/bridge_api.py
- ml/windy-inference/server/daemon.py
- ml/windy-inference/inference/worker.py
- ml/windy-inference/inference/shared_memory.py
