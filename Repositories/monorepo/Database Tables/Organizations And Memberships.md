---
type: definition
repo: monorepo
status: growing
lifecycle_stage: persistence
importance: core
last_reviewed: 2026-06-28
aliases:
  - orgs
  - org_memberships
  - user_org_defaults
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/data
---

# Organizations And Memberships

## Objective
Explain how org identity, user membership, roles, and default-org bootstrapping are represented in the database.

## Description
### What
`orgs`, `org_memberships`, and `user_org_defaults` connect Supabase-auth users to Keyframe product organizations.

### Why
Most product rows are org-scoped. Dashboard access, API key visibility, embed configs, billing, reservations, and plan overrides depend on knowing the caller's org and role.

### When
These tables are used during dashboard auth, org loading, API key creation, role checks, billing lookup, and RLS policy evaluation.

### How
The frontend calls `ensure_default_org` after login. The SQL function creates an org and owner membership when needed, stores a default org, and returns the org id. Backend routes use user-scoped Supabase clients to check membership and service-role clients for privileged writes.

## Scope
| Table | Meaning |
| --- | --- |
| `orgs` | Organization name, plan, and Stripe customer linkage |
| `org_memberships` | User-to-org role: owner, admin, or member |
| `user_org_defaults` | User's default org id |

## Use Case
Read before changing auth, org switching, RLS policies, API-key creation permissions, dashboard org context, or billing ownership.

## Stage In Lifecycle
Persistence and authorization.

## Importance
Core. Most table access fans out from org membership.

## Risks
- Service-role writes can bypass the same RLS constraints seen by dashboard users.
- Role checks must use the caller's JWT-scoped client when proving user permissions.
- Changing default-org behavior can create duplicate orgs or orphan dashboard state.

## Input / Output
| Input | Output |
| --- | --- |
| Supabase user JWT or auth user id | Org id, membership role, and default-org row |

## Resources
- [[Database Tables/Database Table Index]]
- [[Technologies/Postgres RLS]]
- [[Workflows/API Key And Auth Flow]]

## Related Topics
- [[Database Tables/API Keys Table]]
- [[Database Tables/Embed Configs Table]]
- [[Database Tables/Billing Tables]]
- [[Concepts/Production Database Safety]]

## Evidence
- supabase-kfl/supabase/migrations/20260109013713_orgs.sql
- platform/src/features/auth/context/AuthProvider.tsx
- platform/src/features/org/context/OrgProvider.tsx
- modal_kflapi/app.py
