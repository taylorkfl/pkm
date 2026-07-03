---
type: definition
repo: monorepo
status: growing
lifecycle_stage: persistence
importance: core
last_reviewed: 2026-06-28
aliases:
  - embed_configs
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/data
---

# Embed Configs Table

## Objective
Explain how `embed_configs` stores publishable widget configuration, draft state, provider secrets, and runtime voice-agent settings.

## Description
### What
`embed_configs` is the main persisted deployment/config table for embeddable agents. It contains org ownership, publishable key/hash, status, persona id, voice-agent JSON config, encrypted provider key, allowed origins, draft linkage, and audit fields.

### Why
The browser can hold a publishable key but not raw provider credentials. This table lets the backend resolve a safe public key into a private runtime configuration.

### When
Used by dashboard builder flows, create/update/publish/deactivate endpoints, embed session creation, KFL voice-agent runtime config lookup, and draft preview sessions.

### How
Active rows have publishable keys. Draft rows have null keys until publication. The backend encrypts `voice_agent_key_ciphertext`, validates voice-agent JSON, and mints short-lived provider credentials or signed URLs when a session starts.

## Scope
| Concern | Database Contract |
| --- | --- |
| Public lookup | `publishable_key` and `pk_hash_b64` identify active configs |
| Drafts | `status = draft` and `source_config_id` support staging edits |
| Persona binding | `persona_id` selects avatar persona |
| Voice agent | `voice_agent` JSON discriminates `elevenlabs`, `openai`, or `kfl` |
| Secret storage | `voice_agent_key_ciphertext` stores encrypted provider key |
| Origin guard | `allowed_origins` limits where publishable configs can be embedded |

## Use Case
Read before editing widget builder behavior, hosted agent settings, provider credentials, draft publication, preview sessions, or origin restrictions.

## Stage In Lifecycle
Persistence, configuration, and runtime admission.

## Importance
Core. It connects customer-facing widget setup to runtime session creation.

## Risks
- Publishable keys are public, but provider credentials are not.
- Draft-to-active transitions are constrained by key consistency checks.
- Changing `voice_agent` validation can break dashboard publishing and runtime creation.
- Origin restrictions are part of the embed security boundary.

## Input / Output
| Input | Output |
| --- | --- |
| Dashboard config form or preset | Draft or active embed config row |
| Publishable key at session start | Persona, voice-agent config, encrypted key, and allowed origins |

## Resources
- [[Database Tables/Database Table Index]]
- [[Concepts/Embed Config]]
- [[Workflows/Embed Config Builder Lifecycle]]
- [[Concepts/Dynamic Variables]]

## Related Topics
- [[Database Tables/Personas Table]]
- [[Concepts/Publishable Key And Secret Key]]
- [[Concepts/Per-Session Context]]
- [[Concepts/Production Database Safety]]

## Evidence
- supabase-kfl/supabase/migrations/20260124212220_embed_configs.sql
- supabase-kfl/supabase/migrations/20260201100140_add_vapi_and_metadata.sql
- supabase-kfl/supabase/migrations/20260414120000_embed_config_drafts.sql
- supabase-kfl/supabase/migrations/20260414173000_embed_config_edit_drafts.sql
- modal_kflapi/app.py
- platform/src/pages/agents-and-deployments.tsx
- platform/src/features/agents/useWidgetBuilderStore.ts
- kfl-voice-agent/internal/voiceagent/embed_config.go
