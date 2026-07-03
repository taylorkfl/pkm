---
type: definition
repo: monorepo
status: growing
lifecycle_stage: operations
importance: core
last_reviewed: 2026-06-28
aliases:
  - nodes
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/data
---

# Nodes Table

## Objective
Explain how `nodes` represents available GPU avatar runtime capacity.

## Description
### What
`nodes` stores GPU node identity, namespace, state, max capacity, heartbeat timestamps, lease expiry, provider id, and node URL.

### Why
The central API must choose a healthy GPU node for each realtime avatar session without relying on in-memory state inside one API worker.

### When
Used when GPU workers register/deregister, send heartbeats, and when session admission asks for candidate nodes.

### How
Workers upsert node rows when they start, periodically update heartbeat and lease expiry, and mark themselves ready or quarantined. The `find_candidate_nodes` RPC returns ready, leased nodes in a requested namespace.

## Scope
| Field Group | Meaning |
| --- | --- |
| Routing | `namespace`, `url`, `provider_id` |
| Capacity | `max_capacity` |
| Health | `state`, `last_heartbeat_at`, `lease_expires_at`, `quarantined_at` |

## Use Case
Read before changing GPU node registration, autoscaling, session admission, or runtime health checks.

## Stage In Lifecycle
Operations and runtime routing.

## Importance
Core. Without healthy node rows, sessions cannot reserve avatar runtime capacity.

## Risks
- Stale nodes can receive traffic unless leases expire and reapers work.
- Namespace mismatches can make capacity appear unavailable.
- Direct production edits to `url`, `state`, or `max_capacity` can immediately affect live session routing.

## Input / Output
| Input | Output |
| --- | --- |
| Worker heartbeat and registration data | A candidate runtime node for session reservation |

## Resources
- [[Database Tables/Database Table Index]]
- [[Concepts/Reservation And Node Capacity]]
- [[Workflows/Real-Time Persona Session]]

## Related Topics
- [[Database Tables/Reservations Table]]
- [[Concepts/Production Database Safety]]

## Evidence
- supabase-kfl/supabase/migrations/20260109162055_nodes.sql
- supabase-kfl/supabase/migrations/20260109164203_nodes_and_reservations_rpcs.sql
- ml/windy-inference/scripts/register_node.py
- ml/windy-inference/scripts/deregister_node.py
- ml/windy-inference/server/bridge_api.py
- modal_kflapi/app.py
