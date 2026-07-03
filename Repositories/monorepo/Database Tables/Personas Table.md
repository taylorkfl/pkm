---
type: definition
repo: monorepo
status: growing
lifecycle_stage: persistence
importance: core
last_reviewed: 2026-06-28
aliases:
  - personas
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/data
---

# Personas Table

## Objective
Explain how `personas` maps product-facing avatar choices to runtime model and asset metadata.

## Description
### What
`personas` stores persona slug, display name, skin, model id, visibility, optional org ownership, cover asset URI, and deprecation state.

### Why
Session creation needs to resolve a user-visible persona into the model namespace and runtime assets that the GPU worker can load.

### When
Used by dashboard persona pickers, hosted widget presets, API session creation, plugin sessions, and worker persona skin lookup.

### How
Rows are either public or org-scoped. The API resolves persona id or slug against visibility and org, extracts `model_id`, then uses it for plan eligibility and GPU node namespace selection.

## Scope
| Field Group | Meaning |
| --- | --- |
| Identity | `id`, `slug`, `display_name` |
| Runtime routing | `skin`, `model_id` |
| Access | `visibility`, `org_id`, RLS policy |
| UI assets | `cover_asset_uri`, `deprecated` |

## Use Case
Read before changing persona availability, model routing, public/org persona visibility, or demo persona presets.

## Stage In Lifecycle
Persistence and runtime routing.

## Importance
Core. A persona row determines which avatar model/runtime path a session uses.

## Risks
- Changing `model_id` can route sessions to a different node namespace.
- Changing visibility or org ownership can hide personas from customers.
- Cover asset URIs are external assets, not stored directly in this table.

## Input / Output
| Input | Output |
| --- | --- |
| Persona id or slug plus org id | Visible persona row with model and asset metadata |

## Resources
- [[Database Tables/Database Table Index]]
- [[Concepts/Persona]]
- [[Workflows/Real-Time Persona Session]]

## Related Topics
- [[Database Tables/Reservations Table]]
- [[Database Tables/Embed Configs Table]]
- [[Concepts/Production Database Safety]]

## Evidence
- supabase-kfl/supabase/migrations/20260122233251_personas.sql
- supabase-kfl/supabase/migrations/20260323120000_deprecate_persona1.sql
- modal_kflapi/app.py
- platform/src/pages/persona-library.tsx
- ml/windy-inference/server/bridge_api.py
