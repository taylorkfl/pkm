---
type: definition
repo: monorepo
status: growing
lifecycle_stage: design
importance: high
last_reviewed: 2026-06-28
aliases:
  - kfl_voices
  - kfl_llm_models
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/data
---

# KFL Catalog Tables

## Objective
Explain how `kfl_voices` and `kfl_llm_models` provide selectable voice and model options for hosted KFL agents.

## Description
### What
`kfl_voices` and `kfl_llm_models` are catalog tables used by the widget builder and hosted agent flows. They store provider, display metadata, sort order, active state, and voice tuning settings.

### Why
The dashboard needs database-driven options for voices and LLM models without hardcoding every available runtime choice into UI code.

### When
Used when loading the widget builder catalog, creating default widget drafts, applying presets, and building KFL voice-agent config.

### How
Authenticated users can read active rows. Seed scripts populate core catalog data. UI code reads active voices/models and writes selected values into `embed_configs.voice_agent`.

## Scope
| Table | Meaning |
| --- | --- |
| `kfl_voices` | TTS voice id, provider, label, tags, description, and voice settings |
| `kfl_llm_models` | LLM model id, provider, label, description, and sort order |

## Use Case
Read before changing widget-builder voice/model choices, provider defaults, preset behavior, or voice tuning settings.

## Stage In Lifecycle
Design and runtime configuration.

## Importance
High. These tables control the customer-facing KFL agent configuration menu.

## Risks
- Inactive or missing catalog rows can break builder defaults.
- Voice settings shape must remain compatible with KFL voice-agent runtime config.
- Production catalog edits affect newly created or edited deployments.

## Input / Output
| Input | Output |
| --- | --- |
| Active catalog rows | Voice/model picker options and default KFL config values |

## Resources
- [[Database Tables/Database Table Index]]
- [[Database Tables/Embed Configs Table]]
- [[Workflows/Embed Config Builder Lifecycle]]

## Related Topics
- [[Concepts/Per-Session Context]]
- [[Concepts/Production Database Safety]]

## Evidence
- supabase-kfl/supabase/migrations/20260414170000_kfl_voice_and_llm_catalog.sql
- supabase-kfl/supabase/migrations/20260415100000_kfl_voices_voice_settings.sql
- supabase-kfl/supabase/data/seed-core.ts
- platform/src/features/agents/WidgetBuilder.tsx
- platform/src/pages/agents-and-deployments.tsx
