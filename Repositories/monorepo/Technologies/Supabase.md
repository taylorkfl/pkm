---
type: technology
repo: monorepo
status: growing
lifecycle_stage: persistence
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/technology
  - pkm/domain/data
---

# Supabase

## Objective
Explain how Supabase is used in this repo, why it is present, when it runs, and what contracts it affects.

## Description
### What
Postgres-backed backend used for auth-aware data, migrations, RLS, RPCs, and generated types.

### Why
It centralizes product state and atomic database logic, especially org access and reservation capacity.

### When
Used by dashboard, APIs, KFL runtime config lookup, tests, and seed scripts.

### How
Read the repo files listed in Evidence first, then use the external source links to understand the underlying technology behavior. Treat external docs as support for protocol/framework behavior and repo evidence as the source of truth for local implementation.

## Scope
| Lens | What To Learn |
| --- | --- |
| Repo role | Which workflow depends on this technology |
| Contract surface | Types, events, endpoints, tables, tokens, or commands it shapes |
| Failure modes | What breaks when the technology boundary is misunderstood |
| External basis | Official docs, standards, or papers that explain the underlying mechanism |

## Use Case
Use when a workflow note links here or when you encounter this technology in code and need the repo-specific mental model.

## Stage In Lifecycle
Persistence

## Importance
Core. This technology materially affects repo behavior in its domain.

## Risks
Risks include confusing generic technology behavior with this repo’s specific contract, relying on outdated provider docs, or changing one side of a cross-layer interface without updating the others.

## Input / Output
| Input | Output |
| --- | --- |
| Repo usage plus external docs | Technology-specific explanation tied to local evidence |

## Resources
- [[Concepts/Database Migrations And Seeding]]
- [[Workflows/Local Supabase Development And Seeding]]
- [[Database Tables/Database Table Index]]
- [[Database Tables/Database Table Index]]
- [[Concepts/Production Database Safety]]
- Supabase docs: https://supabase.com/docs
- Supabase RLS guide: https://supabase.com/docs/guides/database/postgres/row-level-security

## Related Topics
- [[Technologies/Technology Index]]
- [[Architecture]]
- [[Concepts/Research Source Index]]
- [[Workflows/Local Development And Docker]]

## Evidence
- supabase-kfl/supabase/migrations
- modal_kflapi/app.py
- platform/src/features/org/context/OrgProvider.tsx
