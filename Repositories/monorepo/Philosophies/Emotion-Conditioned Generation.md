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

# Emotion-Conditioned Generation

## Objective
Explain the architectural/research philosophy of Emotion-Conditioned Generation as it appears in this repo.

## Description
### What
Use explicit affect/control signals to shape avatar expression and potentially generated motion.

### Why
Emotion is a compact bridge between language/voice reasoning and avatar expression control.

### When
Use this philosophy when evaluating design tradeoffs in the related workflow or concept notes.

### How
Repo currently carries coarse emotion labels through adapter events and LiveKit control messages.

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
- js-sdk/src/PersonaSession.ts
- kfl-voice-agent/internal/voiceagent/llm.go
