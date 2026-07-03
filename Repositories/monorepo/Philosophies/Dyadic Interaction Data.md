---
type: philosophy
repo: monorepo
status: growing
lifecycle_stage: research
importance: medium
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/philosophy
  - pkm/domain/research
---

# Dyadic Interaction Data

## Objective
Explain the architectural/research philosophy of Dyadic Interaction Data as it appears in this repo.

## Description
### What
Understand conversation quality as a two-party timeline rather than isolated audio generation.

### Why
Turn-taking, interruption, response timing, and expression appropriateness require paired user/assistant signals.

### When
Use this philosophy when evaluating design tradeoffs in the related workflow or concept notes.

### How
Use when designing evaluation data or logs for conversational avatar behavior.

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
Medium. The idea shapes how multiple files should be interpreted together.

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
- kfl-voice-agent/internal/voiceagent/session_core.go
- kfl-voice-agent/internal/voiceagent/stt.go
