---
type: definition
repo: monorepo
status: growing
lifecycle_stage: persistence
importance: core
last_reviewed: 2026-06-28
aliases:
  - api_keys
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/data
---

# API Keys Table

## Objective
Explain how `api_keys` stores server API keys and how those keys gate product API access.

## Description
### What
`api_keys` stores hashed Keyframe secret keys, public key ids, fingerprints, display hints, org ownership, environment, status, scopes, and revocation metadata.

### Why
The central API needs a durable, revocable, org-scoped credential model without storing plaintext secret keys.

### When
Used when dashboard users create/list/revoke API keys and when API clients call session or plugin endpoints with bearer keys.

### How
The backend creates a plaintext key once, stores an Argon2 hash plus SHA-256 fingerprint, and returns the plaintext only in the create response. Later requests compute the fingerprint, load the active row, verify the hash, and attach org/plan context to the request.

## Scope
| Behavior | Database Contract |
| --- | --- |
| Create key | Insert one hashed row with org/user ownership |
| Verify key | Match environment, active status, fingerprint, expiry, and hash |
| Revoke key | Update status to `revoked`; triggers enforce revocation consistency |
| Dashboard listing | RLS allows org owners/admins and limited member-owned rows |

## Use Case
Read before changing API auth, key prefixes, dashboard key UI, revocation behavior, or production key cleanup.

## Stage In Lifecycle
Persistence and authorization.

## Importance
Core. Broken key handling can block customers or expose privileged API access.

## Risks
- Plaintext keys are only returned once and should never be inserted into notes, logs, or migrations.
- Key rows are protected by triggers; direct production edits can fail or create inconsistent state.
- Revoking keys in production can immediately break customers and demos.

## Input / Output
| Input | Output |
| --- | --- |
| Plaintext API key at creation | Stored hash/fingerprint row plus one-time plaintext response |
| Bearer API key at request time | Org id, API key id, plan, and org creation timestamp |

## Resources
- [[Database Tables/Database Table Index]]
- [[Workflows/API Key And Auth Flow]]
- [[Concepts/Publishable Key And Secret Key]]

## Related Topics
- [[Database Tables/Organizations And Memberships]]
- [[Database Tables/Reservations Table]]
- [[Concepts/Production Database Safety]]

## Evidence
- supabase-kfl/supabase/migrations/20260109014008_api_keys.sql
- modal_kflapi/app.py
- platform/src/pages/api-keys.tsx
- modal_kflapi_tests/test_api_key_creation.py
