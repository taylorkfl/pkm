---
type: concept
repo: monorepo
status: growing
lifecycle_stage: persistence
importance: core
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/data
---

# Database Migrations And Seeding

## Objective
Explain migrations, seeding, starting data, auth users, and related database objects in the Supabase-backed repo.

## Description
Migrations are versioned SQL changes that evolve the database schema and behavior. Seeding inserts starter data required for local/dev/test use. Seeds in this repo are not cryptographic seeds; they are JSON-backed records such as auth users, KFL voice catalog rows, and LLM model catalog rows.

## Scope
| Term | Repo Meaning |
| --- | --- |
| Migration | Ordered SQL file under `supabase-kfl/supabase/migrations` |
| Seed | Starter data loaded by seed scripts from `supabase-kfl/supabase/data/seeds` |
| Auth user | Supabase Auth-managed user, created by admin API in `seed-core.ts` and test fixtures |
| Index | Database lookup accelerator, often important for API performance |
| Constraint | Database rule such as uniqueness, required field, foreign key, or valid value |
| Function | Reusable Postgres logic |
| RPC | Postgres function exposed as an app-callable operation |
| Trigger | Automatically-run database logic before/after table changes |

## Use Case
Use this concept before running local Supabase, changing schema, adding seed data, or trying to understand why app code calls a database function instead of writing several tables itself.

## Stage In Lifecycle
Persistence and local development.

## Importance
Core. The app's API, auth, billing, reservations, embed configs, and catalog data depend on stable Supabase contracts.

## Risks
- Local seed files can contain sensitive test identities or credentials if misused; never copy real secrets into seed files.
- Running seed/reset against the wrong Supabase project can mutate shared or production data.
- Migrations and generated types can drift if schema changes are not applied consistently.
- Seed scripts use service-role style access and should be run with the intended environment.

## Input / Output
| Input | Output |
| --- | --- |
| SQL migration files | Updated local/remote database schema and functions |
| Seed JSON plus `ENV=local pnpm run seed:core` | Inserted/upserted starter rows and auth users |
| Supabase local env values | Local API/DB/Auth setup usable by app and tests |

## Resources
- [[Workflows/Local Supabase Development And Seeding]]
- [[Technologies/Supabase]]
- [[Technologies/SQL Migrations]]
- [[Definitions/Database RPC]]
- [[Definitions/Race Condition]]
- [[Database Tables/Database Table Index]]

## Related Topics
- [[Concepts/Production Database Safety]]
- [[Concepts/Reservation And Node Capacity]]
- [[Structure/supabase-kfl/supabase/supabase]]

## Evidence
- supabase-kfl/supabase/config.toml
- supabase-kfl/supabase/package.json
- supabase-kfl/supabase/data/seed-core.ts
- supabase-kfl/supabase/data/plan.ts
- supabase-kfl/supabase/data/utils.ts
- supabase-kfl/supabase/data/seeds
- supabase-kfl/supabase/migrations
- modal_kflapi_tests/conftest.py
