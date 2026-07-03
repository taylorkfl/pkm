---
type: definition
repo: monorepo
status: seedling
lifecycle_stage: persistence
importance: medium
last_reviewed: 2026-06-28
aliases:
  - room_capacity
  - connection_events
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/data
---

# Legacy Capacity Tables

## Objective
Explain older or auxiliary capacity tables that still exist in migrations but are not the primary current runtime path.

## Description
### What
`room_capacity` and `connection_events` define an older room-slot capacity model and connection attempt ledger.

### Why
They appear in migrations, generated types, and utility scripts, so they can confuse code readers who are tracing the newer node/reservation capacity path.

### When
Use this note when you encounter `acquire_room_slot`, `release_room_slot`, `reconcile_room_slots`, `room_capacity`, or `connection_events`.

### How
The table and RPCs model a namespace-level slot counter. Current realtime persona sessions instead use `nodes`, `reservations`, `find_candidate_nodes`, and `reserve_capacity`.

## Scope
| Table Or RPC | Meaning |
| --- | --- |
| `room_capacity` | Namespace-level max/used room counters |
| `connection_events` | Connection attempt queue/status ledger |
| `acquire_room_slot` | Increment used room count if capacity exists |
| `release_room_slot` | Decrement used room count |
| `reconcile_room_slots` | Reconcile counter state |

## Use Case
Read before deleting old migrations, regenerating types, or interpreting older capacity code.

## Stage In Lifecycle
Persistence and legacy operations.

## Importance
Medium. These contracts exist in schema history but are not the primary current path found in active session code.

## Risks
- Treating legacy capacity tables as current can lead to wrong debugging conclusions.
- Utility scripts may still mutate `room_capacity`.
- Confirm active usage before deleting or migrating away from these objects.

## Input / Output
| Input | Output |
| --- | --- |
| Namespace capacity check | Slot counter update or connection attempt row |

## Resources
- [[Database Tables/Database Table Index]]
- [[Database Tables/Nodes Table]]
- [[Database Tables/Reservations Table]]
- [[Concepts/Reservation And Node Capacity]]

## Related Topics
- [[Concepts/Production Database Safety]]
- [[Technologies/SQL Migrations]]

## Evidence
- supabase-kfl/supabase/migrations/20251118195728_tables_room_capacity.sql
- supabase-kfl/supabase/migrations/20251118204749_rpc_acquire_room_slot.sql
- supabase-kfl/supabase/migrations/20251118205321_rpc_release_room_slot.sql
- supabase-kfl/supabase/migrations/20251119182458_rpc_reconcile_room_slots.sql
- supabase-kfl/supabase/migrations/20251122194011_tables_connection_events.sql
- supabase-kfl/supabase/utils/bump-capacity.ts
