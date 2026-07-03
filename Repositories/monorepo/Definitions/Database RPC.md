---
type: definition
repo: monorepo
status: growing
lifecycle_stage: persistence
importance: high
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/data
---

# Database RPC

## Objective
Define the repo's database RPC pattern: a Postgres function exposed as an application-callable operation.

## Description
A database RPC is a callable Postgres function. The app calls it like an operation rather than manually performing several separate table reads/writes. This is useful when the work must happen atomically, such as checking capacity and reserving a runtime slot without allowing two concurrent requests to overbook the same capacity.

## Scope
| Included | Excluded |
| --- | --- |
| Supabase/Postgres functions used as app operations | Browser-to-worker LiveKit RPC topics |
| Capacity/reservation operations that need transactions | Generic HTTP endpoints |
| Database-side consistency and locking | Client-only state updates |

## Use Case
Use database RPCs when a workflow needs correctness under concurrency, permission boundaries, or transactional updates.

## Stage In Lifecycle
Persistence and runtime.

## Importance
High. RPCs are one of the main tools for avoiding [[Definitions/Race Condition|race conditions]] in reservation and capacity logic.

## Risks
- Hiding too much logic in SQL can make behavior harder to discover unless notes and tests link to the function.
- RPC permissions and RLS interactions must be reviewed carefully.
- App code must treat RPC failure as part of the workflow, not as an impossible state.

## Input / Output
| Input | Output |
| --- | --- |
| Application call with parameters | Database-side function result and committed state change |

## Resources
- [[Concepts/Reservation And Node Capacity]]
- [[Technologies/Supabase]]
- [[Technologies/Postgres RLS]]
- [[Concepts/Database Migrations And Seeding]]

## Related Topics
- [[Definitions/Race Condition]]
- [[Concepts/Production Database Safety]]
- [[Workflows/Billing Usage And Session Limits]]

## Evidence
- supabase-kfl/supabase/migrations/20251118204749_rpc_acquire_room_slot.sql
- supabase-kfl/supabase/migrations/20251118205321_rpc_release_room_slot.sql
- supabase-kfl/supabase/migrations/20251119182458_rpc_reconcile_room_slots.sql
- supabase-kfl/supabase/migrations/20260109164203_nodes_and_reservations_rpcs.sql
- modal_kflapi/app.py
- modal_kflapi_tests/test_concurrency_limits.py
