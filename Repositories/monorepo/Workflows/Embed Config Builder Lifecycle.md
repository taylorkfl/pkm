---
type: workflow
repo: monorepo
status: growing
lifecycle_stage: design
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/workflow
  - pkm/domain/product
---

# Embed Config Builder Lifecycle

## Objective
Explain how dashboard widget configuration moves from editable draft to published embed artifact, and why the lifecycle is intentionally stricter than a normal settings form.

## Description
### What
Embed configs are deployable widget artifacts. A draft is mutable and has no publishable key. A published config is active, has a public key and hash, and can start sessions. A disabled config keeps its key identity but is filtered out of session creation. Linked edit drafts stage changes against an already-active config.

### Why
The lifecycle separates editing from deployment. It avoids accidental live changes, preserves public key identity, and keeps the browser-facing embed key stable while allowing controlled updates. It also ensures raw provider keys are encrypted at rest and converted to ephemeral/signed credentials only when a session starts.

### When
It runs in the dashboard widget builder when a user creates a hosted agent, edits a published widget, previews a draft, publishes a draft, deactivates a live config, or discards changes.

### How
Browser-side store logic builds a deterministic draft payload from form fields. It serializes that payload to detect pending changes, saves drafts opportunistically, creates an edit draft only when a live config needs mutation, and publishes by calling the server. Server-side code enforces owner/admin membership, validates persona visibility, normalizes origins, rejects direct edits to active configs, and assigns a publishable key only during publish.

## Scope
| State | Key Behavior | Why It Matters |
| --- | --- | --- |
| Draft | Mutable row, no publishable key/hash | Safe editing and preview without public deployment |
| Linked edit draft | Draft row with `source_config_id` | Stages changes to active configs without mutating live traffic |
| Active | Has publishable key/hash and can create sessions | Public deployment artifact |
| Disabled | Keeps key but is filtered from session lookup | Allows pause/reactivation without rotating embed code |

## Use Case
Use when changing widget builder save/publish/deactivate UX, embed config API routes, origin validation, draft preview, or key handling.

## Stage In Lifecycle
Design and runtime. The builder is design-time, but published configs are runtime session roots.

## Importance
Core. It is the product path from dashboard configuration to customer-facing embeds.

## Risks
- Direct active edits would make live widgets change while the user is still editing.
- Missing allowed origins are rejected on publish because public embeds need an origin policy.
- Changing voice-agent type without key rotation is rejected because old secrets may not match the new provider.
- A stale linked draft can conflict with a changed or disabled source config.
- Key mutation triggers and constraints are important; losing key identity breaks deployed embed code.

## Input / Output
| Input | Output |
| --- | --- |
| Dashboard form values: label, persona, voice, prompt, greeting, STT, origins | Draft payload and saved `embed_configs` row |
| Publish action | Active config with `kfl_pk_live_...` publishable key and hash |
| Deactivate action | Disabled config preserving publishable key |
| Preview request | Session credentials using draft config source when applicable |

## Resources
- [[Concepts/Embed Config]]
- [[Concepts/Publishable Key And Secret Key]]
- [[Concepts/Voice Agent Details]]
- [[Technologies/Postgres RLS]]
- [[Technologies/SQL Migrations]]
- [[Technologies/React]]
- PostgreSQL constraints/triggers reference: https://www.postgresql.org/docs/current/trigger-definition.html
- Supabase RLS guide: https://supabase.com/docs/guides/database/postgres/row-level-security

## Related Topics
- [[Philosophies/Publishable-Key Embed Model]]
- [[Philosophies/Layered SDK Abstraction]]
- [[Workflows/Real-Time Persona Session]]
- [[Structure/platform/platform]]
- [[Structure/modal_kflapi/modal_kflapi]]

## Evidence
- platform/src/features/agents/useWidgetBuilderStore.ts
- platform/src/features/agents/WidgetBuilder.tsx
- platform/src/features/agents/HostedConfigEditor.tsx
- modal_kflapi/app.py
- modal_common/agents.py
- supabase-kfl/supabase/migrations/20260124212220_embed_configs.sql
- supabase-kfl/supabase/migrations/20260414120000_embed_config_drafts.sql
- supabase-kfl/supabase/migrations/20260414173000_embed_config_edit_drafts.sql
- modal_kflapi_tests/test_embed_config_drafts.py
- modal_kflapi_tests/test_embed_session.py
