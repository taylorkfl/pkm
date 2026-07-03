---
type: philosophy
repo: monorepo
status: growing
lifecycle_stage: operations
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/philosophy
  - pkm/domain/security
---

# Publishable-Key Embed Model

## Objective
Explain the architectural/research philosophy of Publishable-Key Embed Model as it appears in this repo.

## Description
### What
Use a browser-safe public key to identify active embed configs while secrets remain server-side.

### Why
It enables easy customer installation without exposing provider keys or general API authority.

### When
Use this philosophy when evaluating design tradeoffs in the related workflow or concept notes.

### How
Publishable key -> hashed active config lookup -> origin validation -> short-lived provider credentials -> session.

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
Operations

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
- modal_kflapi/app.py
- modal_kflapi/crypto.py
- supabase-kfl/supabase/migrations/20260124212220_embed_configs.sql
