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

# js-elements

## Objective
Explain the `js-elements` repo area as part of the larger realtime persona product architecture.

## Description
### What
Embeddable browser layer that creates media elements, fetches embed sessions, captures microphone PCM, connects voice-agent adapters, and wires agent audio to Persona.

### Why
It is the bridge between customer pages and the lower-level SDK. It normalizes third-party and first-party voice providers into a single event contract.

### When
Read this note when a change touches this folder or when a workflow note points here as evidence.

### How
Publishable key -> API session -> PersonaSession -> voice agent adapter -> microphone/audio/control events -> DOM media cleanup.

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
- [[Technologies/ElevenLabs]]

## Related Topics
- [[Workflows/Real-Time Persona Session]]
- [[Workflows/Test-Driven Reading Path]]
- [[Concepts/Research Source Index]]
- [[Concepts/Voice Agent Details]]

## Evidence
- js-elements/src/PersonaEmbed.ts
- js-elements/src/agents/types.ts
- js-elements/src/agents/openai-realtime.ts
- js-elements/src/agents/elevenlabs.ts
- js-elements/src/agents/kfl.ts
- js-elements/src/agents/audio-utils.ts
