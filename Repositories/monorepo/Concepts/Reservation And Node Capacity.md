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
  - pkm/domain/runtime
---

# Reservation And Node Capacity

## Objective
Define reservations as the concurrency, capacity, lifecycle, cleanup, and billing unit for realtime avatar sessions.

## Description
### What
A reservation is the DB-backed claim that an org/session holds a slot on a GPU node. It stores node, org, optional API key, room id, model id, state, deadlines, ready/join/release times, and metering markers.

### Why
GPU capacity is scarce and concurrent. The system cannot rely on in-memory checks in API workers because multiple requests can arrive at the same time. Supabase RPCs use transaction-level locking to make org concurrency and node capacity decisions atomic.

### When
Reservations are created before LiveKit room dispatch and released when the session ends, fails dispatch, times out before ready/user join, exceeds duration, or is reaped by cleanup jobs.

### How
`acquire_gpu_node` asks `find_candidate_nodes` for ready, leased nodes in the model namespace, then loops through candidates and calls `reserve_capacity`. The RPC takes an org advisory lock, counts active org reservations, locks the node row with `for update skip locked`, counts active node reservations, inserts a `reserved` row when capacity exists, and returns structured reasons otherwise.

## Scope
| Reservation State/Field | Meaning |
| --- | --- |
| `released_at is null` | Slot is still considered active for capacity/concurrency |
| `ready_deadline_at` | Startup grace window before stale reservation cleanup |
| `user_join_deadline_at` | Ready room cannot hold capacity forever without user join |
| `max_duration_seconds` | Session duration limit used by room-limit enforcement |
| `model_id` | Billing rate and node namespace connection |
| `meter_reported_at` | Stripe usage reporting completion marker |

## Use Case
Use when investigating 503 busy responses, 429 concurrency responses, stuck sessions, metering bugs, or GPU fleet behavior.

## Stage In Lifecycle
Operations and runtime. Reservation decisions happen at session start; cleanup and metering happen later.

## Importance
Core. It is the bridge between product limits, GPU capacity, LiveKit rooms, and billing.

## Risks
- Counting active reservations by `released_at is null` makes cleanup correctness critical.
- Org concurrency is terminal; trying another node does not fix it.
- Node-level lock failures should try another node because capacity may exist elsewhere.
- If dispatch fails after reservation creation, explicit release prevents phantom capacity usage.
- Reaper safety nets are essential because distributed systems will have partial failures.

## Input / Output
| Input | Output |
| --- | --- |
| Namespace/model, org id, org max concurrent, room id, max duration | Reservation id and selected node URL, or structured no-capacity reason |
| Failed dispatch or timeout | Released reservation and optionally deleted LiveKit room |
| Completed ready/released reservation | Usage credits for billing |

## Resources
- [[Philosophies/Reservation-Based GPU Capacity]]
- [[Workflows/Real-Time Persona Session]]
- [[Workflows/Billing Usage And Session Limits]]
- [[Technologies/Supabase]]
- PostgreSQL explicit locking: https://www.postgresql.org/docs/current/explicit-locking.html
- PostgreSQL advisory locks: https://www.postgresql.org/docs/current/explicit-locking.html#ADVISORY-LOCKS

## Related Topics
- [[Concepts/Billing Credits And Plans]]
- [[Technologies/SQL Migrations]]
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Structure/supabase-kfl/supabase/supabase]]

## Evidence
- modal_kflapi/app.py
- supabase-kfl/supabase/migrations/20260109163200_reservations.sql
- supabase-kfl/supabase/migrations/20260109164203_nodes_and_reservations_rpcs.sql
- supabase-kfl/supabase/migrations/20260210120000_billing.sql
- modal_kflapi_tests/test_session_lifecycle.py
- modal_kflapi_tests/test_concurrency_limits.py
