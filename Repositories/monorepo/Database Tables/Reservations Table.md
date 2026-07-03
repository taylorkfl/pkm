---
type: definition
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
aliases:
  - reservations
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/data
---

# Reservations Table

## Objective
Explain how `reservations` records realtime session capacity, lifecycle state, usage, billing, and cleanup.

## Description
### What
`reservations` is the active and historical session ledger for avatar runtime usage. It links a session to a node, org, optional API key, LiveKit room id, model id, metadata, timing, state, and meter-reporting flags.

### Why
GPU capacity is scarce and billable. The system needs a durable reservation record to coordinate session admission, runtime state transitions, cleanup, usage history, and Stripe meter reporting.

### When
Used before session dispatch, when the avatar worker joins LiveKit, when the source participant joins, when the session ends or fails, during reaper jobs, and when reporting usage.

### How
The `reserve_capacity` SQL RPC inserts a `reserved` row after locking org and node capacity. Runtime code updates states such as `ready`, `in_use`, `ended`, `failed`, or `timed_out`. Scheduled jobs release stale rows, enforce max duration, and report completed non-failed rows to Stripe.

## Scope
| State | Meaning |
| --- | --- |
| `reserved` | DB slot held before node dispatch |
| `dispatching` / `provisioning` | Worker startup path is in progress |
| `ready` | Node joined LiveKit |
| `in_use` | Source user joined LiveKit |
| `ended` | Session finished normally |
| `failed`, `failed_dispatch`, `timed_out` | Failure or cleanup terminal states |

## Use Case
Read before changing session creation, runtime worker state, billing metering, usage UI, capacity limits, or cleanup jobs.

## Stage In Lifecycle
Runtime and billing persistence.

## Importance
Core. It is the durable session lifecycle and usage record.

## Risks
- Leaving `released_at` null holds capacity and can block new sessions.
- Editing `ready_at`, `released_at`, or meter flags can affect billing.
- Reservation state drives voice-agent websocket leases and runtime cleanup.
- Direct production updates can interrupt or misbill live sessions.

## Input / Output
| Input | Output |
| --- | --- |
| Session request, org limits, node capacity, room id | Active reservation row |
| Runtime join/end/failure events | Lifecycle timestamps and released capacity |
| Completed usage | Billing meter report and usage UI rows |

## Resources
- [[Database Tables/Database Table Index]]
- [[Concepts/Reservation And Node Capacity]]
- [[Workflows/Real-Time Persona Session]]
- [[Workflows/Billing Usage And Session Limits]]

## Related Topics
- [[Database Tables/Nodes Table]]
- [[Database Tables/Billing Tables]]
- [[Database Tables/Voice Agent WS Leases Table]]
- [[Concepts/Production Database Safety]]

## Evidence
- supabase-kfl/supabase/migrations/20260109163200_reservations.sql
- supabase-kfl/supabase/migrations/20260109164203_nodes_and_reservations_rpcs.sql
- supabase-kfl/supabase/migrations/20260210120000_billing.sql
- supabase-kfl/supabase/migrations/20260211120000_billing_fixes.sql
- modal_kflapi/app.py
- ml/windy-inference/session.py
- ml/windy-inference/server/daemon.py
- platform/src/pages/usage.tsx
