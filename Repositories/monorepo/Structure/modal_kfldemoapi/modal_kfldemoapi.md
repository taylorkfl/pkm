---
type: structure-note
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: medium
last_reviewed: 2026-06-28
repo_path: modal_kfldemoapi
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/backend
---

# modal_kfldemoapi

## Objective
Explain the demo-specific backend as a separate FastAPI service for pre-call context, post-call scoring, public polling, share rendering, and Lightcone backchannels.

## Description
### What
`modal_kfldemoapi` is the Modal-deployed FastAPI backend for demo experiences: Office Hours, Mock YC Interview, Seed Round Pitch, Job Interview, and YC Lightcone.

### Why
It keeps demo storytelling, scoring, public share cards, and experimental conversation helpers separate from the production session/billing API. That lets demo logic move quickly without changing core session contracts.

### When
It runs before a demo call for context prep, after a call through ElevenLabs post-call webhook scoring, during result polling, and when share links/social bots request previews.

### How
The app avoids env reads at import time, reads secrets at request time, uses `modal_common` for CORS/env/Supabase helpers, validates data with Pydantic, calls OpenAI for context/scoring, verifies ElevenLabs HMAC signatures, persists to `yc_interview_sessions`, and uses public share endpoints for OpenGraph/redirect behavior.

## Scope
| Code Region | Responsibility |
| --- | --- |
| Config/constants | Demo models, TTS/backchannel budgets, upload limits, origins |
| Schemas | Conversation status, scoring result, context prep, backchannel payloads |
| Register/poll | Idempotent session rows and public result fetch |
| Webhook/scoring | Signature verification, transcript formatting, OpenAI structured scoring |
| Context prep | File/URL/notes/resume/application/topic to brief/openers |
| Share | HTML/PNG social unfurl behavior |

## Use Case
Use before changing demo routes, model prompts, scoring schema, public share behavior, or webhook handling.

## Stage In Lifecycle
Runtime. It runs as a deployed demo backend.

## Importance
Medium. Important for demos and public perception, but intentionally separate from core product session APIs.

## Risks
Risks include double-scoring, unverified webhooks, temporary file leaks, stale model IDs, social preview regressions, and mixing demo-only assumptions into product APIs.

## Input / Output
| Input | Output |
| --- | --- |
| Demo request or webhook | Context, score result, stored status, share response, or backchannel audio |

## Resources
- [[Workflows/Demo Scoring And Share Flow]]
- [[Technologies/FastAPI]]
- [[Technologies/Modal]]
- [[Technologies/ElevenLabs]]
- [[Concepts/Research Source Index]]

## Related Topics
- [[Philosophies/Separation Of Product API And Demo API]]
- [[Structure/demo/demo]]
- [[Structure/modal_common/modal_common]]

## Evidence
- modal_kfldemoapi/app.py
- demo/src/lib/kfldemoApi.ts
- supabase-kfl/supabase/migrations/20260430170000_yc_interview_sessions.sql
- supabase-kfl/supabase/migrations/20260512120000_seed_round_pitch_columns.sql
