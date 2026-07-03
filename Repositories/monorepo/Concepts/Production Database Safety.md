---
type: concept
repo: monorepo
status: growing
lifecycle_stage: operations
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/operations
---

# Production Database Safety

## Objective
Explain when to be careful with the production Supabase/Postgres database and why small changes can affect live sessions, billing, and customer access.

## Description
### What
Production database safety means treating Supabase production as a live product control plane, not just storage. It contains org access, API keys, publishable embed configs, personas, reservations, node capacity, voice-agent leases, billing state, Stripe webhook idempotency, catalog pricing, and demo result rows.

### Why
Many runtime decisions are made atomically in the database. A table edit, migration, policy change, or service-role script can change who can start sessions, which config is published, what plan limits apply, how usage is billed, and whether active nodes/reservations clean up correctly.

### When
Be especially careful before doing any of these against production:

| Action | Why It Is Risky |
| --- | --- |
| Running `supabase db push`, `supabase migration up`, `supabase config push`, or any migration command | Can change schema, RLS, triggers, functions, or auth behavior for every user |
| Running seed/setup scripts with production env files | Can overwrite plans, catalog values, personas, or test/demo data |
| Using `SUPABASE_SECRET_KEY` or service-role clients | Bypasses RLS and can read/write across tenants |
| Editing `api_keys` or key hashes/fingerprints | Can revoke or expose customer API access |
| Editing `embed_configs` or drafts | Can publish the wrong widget/voice-agent config |
| Editing `nodes`, `reservations`, `room_capacity`, leases, or reaper-related state | Can strand active sessions, overbook GPUs, or block new sessions |
| Editing billing tables, plan overrides, credit rates, or Stripe processed events | Can overcharge, undercharge, double-process events, or change entitlements |
| Editing webhook handlers and deploying them | Can duplicate provider effects or stop async state updates |
| Changing RLS policies, SQL functions, triggers, or grants | Can open data across orgs or break normal user flows |
| Running destructive commands such as `supabase db reset` or broad deletes/updates | Can wipe or corrupt production data |

### How
Prefer local Supabase for schema iteration, review migrations before applying them, use production env files only in an intentional production deploy context, back up before risky data changes, run narrow queries inside transactions where possible, and verify which project/URL/key is active before executing anything that can mutate data.

## Scope
| Database Area | Production Blast Radius |
| --- | --- |
| `orgs`, `org_memberships`, `user_org_defaults` | User access and ownership |
| `api_keys` | API authentication and revocation |
| `personas`, `embed_configs`, drafts | Customer-facing persona/widget behavior |
| `nodes`, `reservations`, leases | Live capacity and active session routing |
| Billing tables and plan overrides | Entitlements, credits, Stripe state, pricing |
| `kfl_voices`, `kfl_llm_models`, rates | Catalog selection and runtime cost |
| `yc_interview_sessions` | Demo result availability and public share data |

## Use Case
Use this note before touching production Supabase, reviewing a migration, deploying webhook changes, running manual SQL, or using service-role credentials.

## Stage In Lifecycle
Operations and release. It matters most during deploys, migrations, scripts, incident response, and manual support work.

## Importance
Core. The production database is a control plane for product behavior.

## Risks
- Service-role credentials bypass tenant protections. Treat them like production admin access.
- Database functions can encode business logic that application code assumes is atomic.
- Idempotency tables protect against duplicated provider events; editing them can replay side effects.
- Capacity rows describe active infrastructure. Manual cleanup can help incidents, but careless cleanup can terminate or misroute sessions.
- Local commands can point at production if environment variables or Supabase project links are wrong.

## Input / Output
| Input | Output |
| --- | --- |
| Migration, SQL script, CLI command, or service-role operation | Potential production state change |
| Provider webhook event | Durable billing/demo state update |
| Runtime reservation/node mutation | Session admission, dispatch, cleanup, or metering effect |

## Resources
- [[Technologies/Supabase]]
- [[Database Tables/Database Table Index]]
- [[Concepts/Webhooks]]
- [[Workflows/Billing Usage And Session Limits]]
- [[Workflows/Local Development And Docker]]

## Related Topics
- [[Structure/supabase-kfl/supabase/supabase]]
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Structure/modal_kfldemoapi/modal_kfldemoapi]]
- [[Database Tables/Billing Tables]]
- [[Database Tables/Reservations Table]]
- [[Database Tables/Voice Agent WS Leases Table]]

## Evidence
- README.md
- supabase-kfl/supabase/config.toml
- supabase-kfl/supabase/migrations
- supabase-kfl/supabase/load_env.sh
- modal_common/main.py
- modal_kflapi/app.py
- modal_kfldemoapi/app.py
- kfl-voice-agent/internal/voiceagent/ws_lease.go
