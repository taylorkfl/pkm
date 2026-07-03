---
type: structure-note
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/structure
---

# supabase

## Objective
Explain the `supabase` repo area as part of the larger realtime persona product architecture.

## Description
### What
Database schema, RLS policies, seed data, and RPC functions for orgs, personas, API keys, reservations, embed configs, plans, billing, and runtime cleanup.

### Why
Supabase is not just storage. It owns authorization policy, atomic capacity decisions, billing usage calculations, and schema evolution as product contracts.

### When
Read this note when a change touches this folder or when a workflow note points here as evidence.

### How
Migrations define tables/policies/RPCs -> API uses service/user clients -> tests verify behavior -> generated types feed frontend code.

## Scope
| Reading Layer | What To Look For |
| --- | --- |
| Entry points | Which file receives user/API/runtime traffic first |
| Contracts | Request/response types, events, SQL schema, config structs |
| State transitions | How objects move between draft/active, reserved/ready/released, listening/speaking |
| Tests | Negative paths and product rules |

## Use Case
Use as the local map before editing files in this folder or tracing a workflow through it.

## Stage In Lifecycle
Runtime and design. Structure notes connect file layout to behavior.

## Importance
Core structure note for understanding ownership and blast radius.

## Risks
The risk is reading this area in isolation and missing contracts owned by adjacent API, database, SDK, or runtime layers.

## Input / Output
| Input | Output |
| --- | --- |
| Folder/file evidence | Explanation of responsibility, dependencies, and workflow role |

## Resources
- [[Architecture]]
- [[Workflows/Product Learning Path]]
- [[Structure/Structure Index]]
- [[Database Tables/Database Table Index]]
- [[Concepts/Production Database Safety]]

## Related Topics
- [[Workflows/Real-Time Persona Session]]
- [[Workflows/Test-Driven Reading Path]]
- [[Concepts/Research Source Index]]
- [[Technologies/Supabase]]

## Evidence
- supabase-kfl/supabase/migrations
- supabase-kfl/supabase/data
- supabase-kfl/supabase/types_dests.json
- supabase-kfl/supabase/gen_and_update_types.sh
