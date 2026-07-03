---
type: workflow
repo: monorepo
status: growing
lifecycle_stage: operations
importance: high
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/workflow
  - pkm/domain/development
---

# Local Supabase Development And Seeding

## Objective
Document how local Supabase development is wired in this repo and what the local env/seeding commands do.

## Description
The Supabase setup lives in `supabase-kfl/supabase`, not a top-level `supabase` directory. The local workflow is to copy the sample env file, source it, start Supabase, apply migrations, and run the current seed command when starter data is needed.

## Scope
| Step | Meaning |
| --- | --- |
| `cp .env.supabase.local.sample .env.supabase.local` | Create local-only env file |
| `source load_env.sh .env.supabase.local` | Export env vars into the current shell |
| `supabase start` | Start local Supabase services using `config.toml` |
| `supabase migration up` | Apply pending local migrations |
| `ENV=local pnpm run seed:core` | Run `dotenvx` with `.env.supabase.local` and execute `data/seed-core.ts` |
| `SUPABASE_API_URL` | Local or remote Supabase API URL used by scripts/API code |
| `SUPABASE_SECRET_KEY` | Service-role style key for admin seed/test operations; do not expose to browser |
| `AUTH_SITE_URL` | Base app URL used by Supabase Auth redirects |

## Use Case
Use this workflow when preparing a local database for frontend/API testing or when understanding seed/migration commands from the root README.

## Stage In Lifecycle
Operations and local development.

## Importance
High. Most product workflows depend on Supabase state, and unsafe env configuration can affect the wrong database.

## Risks
- Sourced variables only affect the current shell and child processes; they do not persist across reboot or unrelated terminals.
- Production should normally use platform/cloud secret managers, not manual shell sourcing.
- Service-role keys must not be committed or placed in frontend env files.
- Browser cache/local storage can hold stale auth state after reseeding/reset.

## Input / Output
| Input | Output |
| --- | --- |
| `.env.supabase.local` | Environment variables exported into current shell |
| Supabase CLI config and migrations | Local database/auth services |
| `ENV=local pnpm run seed:core` | Auth users plus KFL catalog rows seeded/upserted |

## Resources
- [[Concepts/Database Migrations And Seeding]]
- [[Concepts/Secret Management And Environment Variables]]
- [[Technologies/Supabase]]
- [[Structure/supabase-kfl/supabase/supabase]]

## Related Topics
- [[Concepts/Auth Redirects And Magic Links]]
- [[Workflows/Cloudflared Tunnel Development]]
- [[Concepts/Production Database Safety]]

## Evidence
- README.md
- supabase-kfl/supabase/.env.supabase.local.sample
- supabase-kfl/supabase/load_env.sh
- supabase-kfl/supabase/config.toml
- supabase-kfl/supabase/package.json
- supabase-kfl/supabase/data/seed-core.ts
- supabase-kfl/supabase/data/utils.ts
