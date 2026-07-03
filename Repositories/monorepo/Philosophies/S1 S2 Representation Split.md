---
type: philosophy
repo: monorepo
status: growing
lifecycle_stage: research
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/philosophy
  - pkm/domain/research
---

# S1 S2 Representation Split

## Objective
Explain the architectural/research philosophy of S1 S2 Representation Split as it appears in this repo.

## Description
### What
Separate high-level communicative state from low-level audiovisual realization.

### Why
The split helps reason about which signals belong to interaction semantics versus synthesis details.

### When
Use this philosophy when evaluating design tradeoffs in the related workflow or concept notes.

### How
Use when mapping transcripts, emotion, prosody, alignment, and avatar motion into research/product boundaries.

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
Research

## Importance
High. The idea shapes how multiple files should be interpreted together.

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
- js-elements/src/agents/types.ts
- kfl-voice-agent/internal/voiceagent/llm.go
- kfl-voice-agent/internal/voiceagent/tts.go
