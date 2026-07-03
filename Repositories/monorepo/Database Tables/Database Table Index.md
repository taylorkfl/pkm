---
type: concept
repo: monorepo
status: growing
lifecycle_stage: persistence
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/data
---

# Database Table Index

## Objective
Index the Supabase/Postgres tables that shape product behavior, runtime coordination, billing, and demos in the monorepo.

## Description
### What
This folder is a navigable map of the database tables and table families used by the repo.

### Why
Most important product behavior crosses application code and database contracts. A route or UI component often only makes sense after reading the table, RLS policy, trigger, or SQL RPC it depends on.

### When
Use this index before changing migrations, dashboard reads, API writes, runtime reservation logic, billing flows, seed data, or demo persistence.

### How
Start from the table family closest to the feature, then follow the related workflow and technology notes. Treat migrations as the source of truth for schema and policies; treat API/frontend/runtime files as evidence for active usage.

## Scope
| Cluster | Tables | Note |
| --- | --- | --- |
| Identity and orgs | `orgs`, `org_memberships`, `user_org_defaults` | [[Database Tables/Organizations And Memberships]] |
| API access | `api_keys` | [[Database Tables/API Keys Table]] |
| Persona catalog | `personas` | [[Database Tables/Personas Table]] |
| Embeds and deployments | `embed_configs` | [[Database Tables/Embed Configs Table]] |
| Runtime capacity | `nodes`, `reservations` | [[Database Tables/Nodes Table]], [[Database Tables/Reservations Table]] |
| Billing | `subscriptions`, `model_credit_rates`, `plan_overrides`, `stripe_processed_events` | [[Database Tables/Billing Tables]] |
| Voice agent session leases | `voice_agent_ws_leases` | [[Database Tables/Voice Agent WS Leases Table]] |
| Builder catalogs | `kfl_voices`, `kfl_llm_models` | [[Database Tables/KFL Catalog Tables]] |
| Demo sessions | `yc_interview_sessions` | [[Database Tables/YC Interview Sessions Table]] |
| Legacy or auxiliary capacity | `room_capacity`, `connection_events` | [[Database Tables/Legacy Capacity Tables]] |

## Use Case
Use this as the table-level entry point for database impact analysis.

## Stage In Lifecycle
Persistence and operations.

## Importance
Core. These tables are the durable product contracts behind dashboard workflows, session admission, runtime cleanup, and billing.

## Risks
- Service-role scripts can bypass RLS and mutate production data quickly.
- Schema, RLS, trigger, or SQL RPC changes can break frontend generated types, backend assumptions, or runtime cleanup.
- Some older tables still exist in migrations but may not be actively used by current application paths.
- Demo-only tables should not be confused with core product session tables.

## Input / Output
| Input | Output |
| --- | --- |
| Feature area, table name, migration, or runtime bug | The table note and adjacent workflow/technology notes to read next |

## Resources
- [[Technologies/Supabase]]
- [[Technologies/Postgres RLS]]
- [[Technologies/SQL Migrations]]
- [[Structure/supabase-kfl/supabase/supabase]]
- [[Concepts/Production Database Safety]]

## Related Topics
- [[Architecture]]
- [[Workflows/Real-Time Persona Session]]
- [[Workflows/Billing Usage And Session Limits]]
- [[Workflows/Embed Config Builder Lifecycle]]
- [[Workflows/API Key And Auth Flow]]

## Evidence
- supabase-kfl/supabase/migrations
- supabase-kfl/supabase/types_dests.json
- modal_kflapi/app.py
- platform/src/features/auth/context/AuthProvider.tsx
- platform/src/pages/agents-and-deployments.tsx
- ml/windy-inference/session.py
- kfl-voice-agent/internal/voiceagent/ws_lease.go
