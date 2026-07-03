---
type: concept
repo: monorepo
status: growing
lifecycle_stage: design
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/product
---

# Embed Config

## Objective
Define embed configs as the persistent deployment records that connect a public widget key to persona, origins, and voice-agent runtime settings.

## Description
### What
An embed config is a row in `embed_configs` that maps an org-owned widget deployment to a persona, voice-agent configuration, encrypted provider key, allowed origins, status, and optional publishable key/hash.

### Why
The widget needs a browser-usable identifier, but the server must own secrets and authorization. The config record gives the browser a stable public handle while keeping provider credentials, persona visibility, and runtime settings server-governed.

### When
Created/edited in the dashboard, published before public use, read by `/v1/embed/create_session`, and optionally fetched by the KFL voice-agent runtime to apply per-config prompt, voice, LLM, STT, and TTS settings.

### How
Draft rows have `status = draft` and no key. Publishing assigns a `kfl_pk_live_...` key and SHA hash. Active rows are looked up by hash and status. Disabled rows preserve key identity but are filtered out. KFL runtime config can be resolved from active publishable key or from signed draft config id during preview.

## Scope
| Field | Conceptual Role |
| --- | --- |
| `publishable_key` | Public widget identity after publish |
| `pk_hash_b64` | Lookup key without querying by raw public value |
| `status` | Draft, active, or disabled lifecycle state |
| `persona_id` | Avatar/model identity for session reservation |
| `voice_agent` | JSONB runtime provider/config payload |
| `voice_agent_key_ciphertext` | Encrypted provider secret or KFL-managed placeholder |
| `allowed_origins` | Browser origin policy for public embed sessions |
| `source_config_id` | Link from edit draft to active source config |

## Use Case
Use when changing hosted agents, publish behavior, widget preview, public embed sessions, or KFL runtime configuration.

## Stage In Lifecycle
Design, persistence, and runtime. It starts as dashboard state and becomes a runtime session root.

## Importance
Core. It is the persisted contract between dashboard configuration and public browser embeds.

## Risks
- Missing origins on publish create unsafe public deployment defaults.
- Direct mutation of active configs can surprise live customers.
- Key identity mutation breaks already-installed embed snippets.
- Voice-agent JSON shape must stay compatible across dashboard, API, and KFL runtime structs.
- Encrypted provider key rotation rules must match provider type changes.

## Input / Output
| Input | Output |
| --- | --- |
| Dashboard form values | Draft or active embed config row |
| Publishable key plus request origin | Active config or rejected session request |
| KFL signed config claim | Runtime voice-agent settings for preview |

## Resources
- [[Workflows/Embed Config Builder Lifecycle]]
- [[Concepts/Publishable Key And Secret Key]]
- [[Concepts/Voice Agent Details]]
- [[Technologies/Postgres RLS]]
- [[Technologies/SQL Migrations]]
- Supabase RLS guide: https://supabase.com/docs/guides/database/postgres/row-level-security

## Related Topics
- [[Philosophies/Publishable-Key Embed Model]]
- [[Structure/platform/platform]]
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Structure/kfl-voice-agent/kfl-voice-agent]]

## Evidence
- modal_kflapi/app.py
- modal_common/agents.py
- platform/src/features/agents/useWidgetBuilderStore.ts
- kfl-voice-agent/internal/voiceagent/embed_config.go
- supabase-kfl/supabase/migrations/20260124212220_embed_configs.sql
- supabase-kfl/supabase/migrations/20260414120000_embed_config_drafts.sql
- supabase-kfl/supabase/migrations/20260414173000_embed_config_edit_drafts.sql
- modal_kflapi_tests/test_embed_config_drafts.py
