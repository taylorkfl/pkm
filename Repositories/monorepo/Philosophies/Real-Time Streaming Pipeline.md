---
type: philosophy
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/philosophy
  - pkm/domain/runtime
---

# Real-Time Streaming Pipeline

## Objective
Explain the architectural/research philosophy of Real-Time Streaming Pipeline as it appears in this repo.

## Description
### What
Design voice/avatar behavior as streams of events and bytes rather than request/response blobs.

### Why
Realtime conversation requires overlapping capture, recognition, generation, synthesis, transport, playback, and cancellation.

### When
Use this philosophy when evaluating design tradeoffs in the related workflow or concept notes.

### How
Mic PCM, STT tokens, LLM tokens, TTS chunks, LiveKit media tracks, and control events all stream independently.

## Scope
| Lens | Question |
| --- | --- |
| What | What pattern is the repo using? |
| Why | What product or runtime pressure makes it valuable? |
| When | Which workflow activates this idea? |
| How | Which files, schemas, events, or tests embody it? |

## Use Case
Use when deciding whether a future change follows or violates the repo’s established design direction.

## Stage In Lifecycle
Runtime

## Importance
Core. The idea shapes how multiple files should be interpreted together.

## Risks
The risk is treating the philosophy as a slogan instead of checking whether a proposed change preserves the concrete contracts in Evidence.

## Input / Output
Not applicable

## Resources
- [[Philosophies/Philosophy Index]]
- [[Architecture]]
- [[Concepts/Research Source Index]]

## Related Topics
- [[Workflows/Product Learning Path]]
- [[Workflows/Test-Driven Reading Path]]
- [[Concepts/Concept Index]]

## Evidence
- js-elements/src/PersonaEmbed.ts
- js-sdk/src/PersonaSession.ts
- kfl-voice-agent/internal/voiceagent/session_core.go
- kfl-voice-agent/internal/voiceagent/llm.go
- kfl-voice-agent/internal/voiceagent/tts.go
