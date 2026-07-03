---
type: definition
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: supporting
last_reviewed: 2026-06-28
aliases:
  - yc_interview_sessions
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/data
---

# YC Interview Sessions Table

## Objective
Explain how `yc_interview_sessions` persists demo conversation status, scoring results, transcripts, and share data.

## Description
### What
`yc_interview_sessions` is a demo-specific table keyed by ElevenLabs `conversation_id`. It stores experience metadata, persona display fields, status, score, quotes, rationale, transcript summary, raw transcript, errors, and scoring timestamps.

### Why
The demo needs a durable join key between the live frontend session, the ElevenLabs post-call webhook, result polling, and public share pages.

### When
Used when the SPA registers a demo call, when the ElevenLabs post-call webhook arrives, when polling for results, and when serving share pages or OG images.

### How
The demo API inserts a row at call start, marks it `scoring` before calling OpenAI, stores either `ready` scoring data or `failed` details, and lets the SPA poll by conversation id.

## Scope
| State | Meaning |
| --- | --- |
| `in_progress` | Demo call is active or awaiting webhook |
| `scoring` | Webhook received and OpenAI scoring is running |
| `ready` | Score/share data is available |
| `failed` | Transcript/scoring failed but the frontend can soft-fail |

## Use Case
Read before changing demo registration, ElevenLabs post-call webhook handling, scoring, polling, or share endpoints.

## Stage In Lifecycle
Runtime and demo persistence.

## Importance
Supporting. It is important for demos but separate from core product session reservations.

## Risks
- It is service-role only; the SPA reads through the API, not directly through Supabase.
- Unknown webhooks are acknowledged to avoid retries; missing registration rows will skip scoring.
- Returning non-200 for handled scoring failures can cause webhook retry loops.

## Input / Output
| Input | Output |
| --- | --- |
| Conversation id and demo metadata | Registered demo session row |
| ElevenLabs post-call transcript | Stored score, transcript summary, raw transcript, and share data |

## Resources
- [[Database Tables/Database Table Index]]
- [[Workflows/Demo Scoring And Share Flow]]
- [[Concepts/Webhooks]]
- [[Concepts/Dynamic Variables]]

## Related Topics
- [[Structure/modal_kfldemoapi/modal_kfldemoapi]]
- [[Technologies/ElevenLabs]]
- [[Concepts/Per-Session Context]]

## Evidence
- supabase-kfl/supabase/migrations/20260430170000_yc_interview_sessions.sql
- modal_kfldemoapi/app.py
- demo/src/lib/kfldemoApi.ts
- demo/src/App.tsx
