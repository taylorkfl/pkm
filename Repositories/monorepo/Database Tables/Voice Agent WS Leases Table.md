---
type: definition
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: high
last_reviewed: 2026-06-28
aliases:
  - voice_agent_ws_leases
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/data
---

# Voice Agent WS Leases Table

## Objective
Explain how `voice_agent_ws_leases` prevents duplicate active KFL voice-agent WebSocket sessions for one reservation.

## Description
### What
`voice_agent_ws_leases` stores one active lease per reservation, keyed by reservation id and signed session nonce, with token expiry, lease expiry, heartbeat, and release metadata.

### Why
The KFL voice agent needs replay protection and single-active-session enforcement for reservation-bound signed WebSocket URLs.

### When
Used when a KFL voice-agent WebSocket opens, heartbeats, validates, closes, or expires.

### How
The Go voice agent calls service-role Supabase RPCs to claim, touch, validate, and release leases. The database uses unique indexes and advisory locking to serialize claims per reservation.

## Scope
| RPC | Meaning |
| --- | --- |
| `claim_voice_agent_ws_lease` | Acquire active lease for reservation/session nonce |
| `touch_voice_agent_ws_lease` | Extend lease during active WebSocket heartbeat |
| `validate_voice_agent_ws_lease` | Confirm session is still valid |
| `release_voice_agent_ws_lease` | Mark lease released on close |
| `reap_expired_voice_agent_ws_leases` | Cleanup expired/inactive leases |

## Use Case
Read before changing KFL signed URL auth, WebSocket leasing, replay prevention, or reservation-bound voice-agent sessions.

## Stage In Lifecycle
Runtime and security.

## Importance
High. It protects active voice-agent sessions and keeps reservations tied to exactly one active WebSocket lease.

## Risks
- Lease TTLs that are too short can close healthy sessions.
- Lease TTLs that are too long can leave stuck sessions active.
- Service-role RPCs bypass normal dashboard RLS and must be handled carefully.

## Input / Output
| Input | Output |
| --- | --- |
| Reservation id, signed session nonce, token expiry | Active or rejected WebSocket lease |

## Resources
- [[Database Tables/Database Table Index]]
- [[Workflows/KFL Voice Agent Session]]
- [[Concepts/Per-Session Context]]

## Related Topics
- [[Database Tables/Reservations Table]]
- [[Concepts/Production Database Safety]]

## Evidence
- supabase-kfl/supabase/migrations/20260407090000_voice_agent_ws_leases.sql
- kfl-voice-agent/internal/voiceagent/ws_lease.go
- kfl-voice-agent/internal/voiceagent/main.go
- modal_common/agents.py
