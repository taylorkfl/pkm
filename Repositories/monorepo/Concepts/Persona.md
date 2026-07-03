---
type: concept
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/product
---

# Persona

## Objective
Define persona as the product identity that links avatar/model selection, org visibility, and session authorization.

## Description
### What
A persona is the entity selected when creating a session. It carries an id/slug, model id, visibility, and optional org ownership. It determines which avatar/model namespace must be reserved.

### Why
Session creation should not let a caller arbitrarily pick model capacity or private avatars. Persona resolution centralizes visibility and maps product identity to runtime model id.

### When
Personas are resolved during API sessions, plugin sessions, KFL sessions, embed sessions, and embed config creation/update.

### How
`resolve_persona` accepts exactly one `persona_id` or scoped `persona_slug`. Public personas can be selected by any org; org personas are scoped to the caller org. The resolved `model_id` is checked against plan `allowed_models` before reservation.

## Scope
| Selector | Behavior |
| --- | --- |
| `persona_id` | Must be UUID and visible as public or org-owned |
| `public:<slug>` | Looks up public persona by slug |
| `org:<slug>` | Looks up org-owned persona by slug |
| `model_id` | Drives plan gate, node namespace, and billing rate |

## Use Case
Use when changing persona libraries, session creation, org-specific avatars, or plan/model access.

## Stage In Lifecycle
Runtime and design. Persona is selected before a session can reserve capacity.

## Importance
Core. It links user-facing avatar choice to runtime model/capacity behavior.

## Risks
- Accepting both id and slug creates ambiguity and is rejected.
- Invalid slug scope should fail early.
- Private org personas must not leak across orgs.
- Plan/model gates must happen after resolution and before reservation.

## Input / Output
| Input | Output |
| --- | --- |
| Persona id or scoped slug plus org id | Visible persona row with model id |
| Persona model id plus plan limits | Allowed session or forbidden model response |

## Resources
- [[Workflows/Real-Time Persona Session]]
- [[Concepts/Reservation And Node Capacity]]
- [[Concepts/Embed Config]]

## Related Topics
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Structure/supabase-kfl/supabase/supabase]]
- [[Technologies/Postgres RLS]]

## Evidence
- modal_kflapi/app.py
- supabase-kfl/supabase/migrations/20260122233251_personas.sql
- supabase-kfl/supabase/data/seeds/personas.base.json
- modal_kflapi_tests/test_create_session.py
