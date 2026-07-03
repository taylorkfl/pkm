---
type: definition
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: high
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/concurrency
---

# Race Condition

## Objective
Define race condition in the context of session capacity, reservations, tests, and realtime state.

## Description
A race condition is a bug where behavior depends on unlucky timing between concurrent operations. In this repo, the clearest example is capacity reservation: if two requests separately check available capacity and both insert reservations before either sees the other's write, the system can overbook a node or org limit.

## Scope
| Race-Prone Area | Why Timing Matters |
| --- | --- |
| Reservation/capacity acquisition | Multiple session requests can arrive at once |
| WebSocket leases | Duplicate voice-agent connections can compete for the same reservation |
| Realtime interruption | User barge-in can arrive while audio is still streaming |
| Test setup/teardown | Shared external resources can be mutated by concurrent tests |

## Use Case
Use this definition when evaluating whether logic should move into a database RPC, transaction, lock, lease, or idempotent handler.

## Stage In Lifecycle
Runtime and testing.

## Importance
High. Realtime session systems and capacity accounting fail quietly when race conditions are not handled.

## Risks
- Local manual testing may miss races because timing rarely lines up.
- App-side check-then-write logic is fragile under concurrency.
- Retried webhook or session requests can re-enter code paths unexpectedly.

## Input / Output
| Input | Output |
| --- | --- |
| Two or more concurrent operations touching shared state | Potentially inconsistent state unless the operation is atomic |

## Resources
- [[Definitions/Database RPC]]
- [[Concepts/Reservation And Node Capacity]]
- [[Concepts/Production Database Safety]]
- [[Workflows/Test-Driven Reading Path]]

## Related Topics
- [[Technologies/Supabase]]
- [[Database Tables/Reservations Table]]
- [[Database Tables/Nodes Table]]
- [[Database Tables/Voice Agent WS Leases Table]]

## Evidence
- supabase-kfl/supabase/migrations/20260109164203_nodes_and_reservations_rpcs.sql
- supabase-kfl/supabase/migrations/20260407090000_voice_agent_ws_leases.sql
- modal_kflapi_tests/test_concurrency_limits.py
- kfl-voice-agent/internal/voiceagent/ws_lease.go
- kfl-voice-agent/internal/voiceagent/session_interrupt_test.go
