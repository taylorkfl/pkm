---
type: philosophy
repo: monorepo
status: growing
lifecycle_stage: design
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/philosophy
  - pkm/domain/architecture
---

# Layered SDK Abstraction

## Objective
Explain the architectural/research philosophy of Layered SDK Abstraction as it appears in this repo.

## Description
### What
Keep low-level session transport, embeddable DOM orchestration, and React/dashboard UX in separate layers.

### Why
It lets customers choose the smallest surface they need and lets internals change without forcing every consumer to learn LiveKit or provider APIs.

### When
Use this philosophy when evaluating design tradeoffs in the related workflow or concept notes.

### How
Low-level `PersonaSession` owns LiveKit; `PersonaEmbed` owns DOM/media/voice-agent wiring; React/platform own UI state and product flows.

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
Design

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
- js-sdk/src/PersonaSession.ts
- js-elements/src/PersonaEmbed.ts
- js-elements/src/agents/types.ts
- platform/src/features/agents/useWidgetBuilderStore.ts
