---
type: philosophy
repo: monorepo
status: growing
lifecycle_stage: design
importance: medium
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/philosophy
  - pkm/domain/architecture
---

# Separation Of Product API And Demo API

## Objective
Explain the architectural/research philosophy of Separation Of Product API And Demo API as it appears in this repo.

## Description
### What
Keep durable product/session/billing APIs separate from demo/scoring APIs.

### Why
Demo flows can iterate on storytelling and scoring without destabilizing customer session and billing contracts.

### When
Use this philosophy when evaluating design tradeoffs in the related workflow or concept notes.

### How
Product API owns sessions/configs/billing; demo API owns demo-specific context and scoring.

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
- modal_kflapi/app.py
- modal_kfldemoapi/app.py
- demo/src/lib/kfldemoApi.ts
