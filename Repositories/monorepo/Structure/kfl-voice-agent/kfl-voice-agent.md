---
type: structure-note
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/structure
---

# kfl-voice-agent

## Objective
Explain the `kfl-voice-agent` repo area as part of the larger realtime persona product architecture.

## Description
### What
Go runtime for first-party STT -> LLM -> TTS voice conversations over WebSocket.

### Why
It gives Keyframe Labs control over turn-taking, endpoint coalescing, interruption, provider abstraction, emotion parsing, and runtime config resolution.

### When
Read this note when a change touches this folder or when a workflow note points here as evidence.

### How
Browser WebSocket audio -> Soniox STT -> endpoint buffer -> OpenRouter SSE -> TTS provider WS -> browser audio/events.

## Scope
| Reading Layer | What To Look For |
| --- | --- |
| Entry points | Which file receives user/API/runtime traffic first |
| Contracts | Request/response types, events, SQL schema, config structs |
| State transitions | How objects move between draft/active, reserved/ready/released, listening/speaking |
| Tests | Negative paths and product rules |

## Use Case
Use as the local map before editing files in this folder or tracing a workflow through it.

## Stage In Lifecycle
Runtime and design. Structure notes connect file layout to behavior.

## Importance
Core structure note for understanding ownership and blast radius.

## Risks
The risk is reading this area in isolation and missing contracts owned by adjacent API, database, SDK, or runtime layers.

## Input / Output
| Input | Output |
| --- | --- |
| Folder/file evidence | Explanation of responsibility, dependencies, and workflow role |

## Resources
- [[Architecture]]
- [[Workflows/Product Learning Path]]
- [[Structure/Structure Index]]

## Related Topics
- [[Workflows/Real-Time Persona Session]]
- [[Workflows/Test-Driven Reading Path]]
- [[Concepts/Research Source Index]]

## Evidence
- kfl-voice-agent/ARCHITECTURE.md
- kfl-voice-agent/internal/voiceagent/session_core.go
- kfl-voice-agent/internal/voiceagent/stt.go
- kfl-voice-agent/internal/voiceagent/llm.go
- kfl-voice-agent/internal/voiceagent/tts.go
- kfl-voice-agent/internal/voiceagent/embed_config.go
- kfl-voice-agent/internal/voiceagent/*_test.go
