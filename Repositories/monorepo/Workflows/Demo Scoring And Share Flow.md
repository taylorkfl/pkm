---
type: workflow
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: medium
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/workflow
  - pkm/domain/product
---

# Demo Scoring And Share Flow

## Objective
Explain how the demo application prepares context, registers conversations, receives post-call transcripts, scores them, polls results, and renders share cards/backchannels.

## Description
### What
The demo flow is a separate product loop from core persona sessions. It prepares conversation context, registers a demo conversation id, waits for an ElevenLabs post-call transcript webhook, scores the transcript with OpenAI structured output, stores the result, lets the SPA poll for readiness, and renders public share pages/images.

### Why
The demo API is intentionally separate from the product API because it can carry marketing/demo-specific behavior: YC interview scoring, seed-round verdicts, job-interview prep, Lightcone podcast prep, backchannel generation, OpenGraph rendering, and public share routes.

### When
It runs before, during, and after demo conversations. Pre-call endpoints create prompt context; registration starts tracking; the webhook moves the session from `in_progress` to `scoring` to `ready` or `failed`; polling retrieves the result; share endpoints serve social previews.

### How
The frontend client wraps form-data and JSON calls. The backend validates payloads with Pydantic, uploads temporary files to OpenAI for context prep, deletes files after use, verifies ElevenLabs webhook HMAC/timestamp, formats transcripts into speaker-tagged text, requests strict JSON scoring from OpenAI, persists score/verdict/interest arc fields, and acknowledges failures with stored `failed` status so the SPA can soft-fail.

## Scope
| Subflow | What Happens | Key Evidence |
| --- | --- | --- |
| Register | Idempotently creates `yc_interview_sessions` row with `in_progress` | `register_session` |
| Poll | Returns public status/result by conversation id | `get_session`, `fetchYcSession` |
| Post-call webhook | Verifies ElevenLabs signature, guards duplicate scoring, stores ready/failed | `elevenlabs_post_call_webhook` |
| Context prep | Converts URLs/files/notes/resumes/applications into agent briefings | `prepare_context`, `prepare_job_interview_context`, seed/Lightcone prep |
| Scoring | Uses strict JSON schema for score, quote, verdict, arc | `_score_with_openai`, `SCORING_JSON_SCHEMA` |
| Share | Bots get OpenGraph HTML/image; humans redirect to SPA | share endpoints in `modal_kfldemoapi/app.py` |
| Backchannel | LLM selects short listener interjections and TTS returns PCM | `requestLightconeBackchannel`, backchannel models |

## Use Case
Use when learning or modifying demo onboarding, scoring, post-call results, share cards, Lightcone backchannels, or job/YC/seed pitch prep.

## Stage In Lifecycle
Runtime. It is a live demo product path but intentionally outside the durable product API.

## Importance
Medium. It does not gate core customer sessions, but it is rich product logic and can shape public perception.

## Risks
- Conversation ids are treated as public unguessable handles; changing that assumption affects polling/share security.
- Webhook signature tolerance and duplicate status guards prevent replay/double-scoring problems.
- Returning 200 on scoring failure prevents endless provider retries but means failures must be visible through polling.
- File uploads are temporary and must be deleted in `finally` paths.
- Strict JSON schema helps scoring reliability but can fail if model/provider behavior changes.
- Demo model IDs and provider availability should be verified before assuming local reproducibility.

## Input / Output
| Input | Output |
| --- | --- |
| Conversation metadata | `yc_interview_sessions` row with `in_progress` |
| ElevenLabs transcript webhook | Stored score, quote, summary, verdict, arc, or failure code |
| URLs/files/notes/resume/application/topic | Context brief and optional first message/openers |
| Share request | Redirect, OpenGraph HTML, or PNG preview |
| Speaker text and alignment timing | Optional backchannel audio and char offset |

## Resources
- [[Structure/demo/demo]]
- [[Structure/modal_kfldemoapi/modal_kfldemoapi]]
- [[Philosophies/Separation Of Product API And Demo API]]
- [[Technologies/OpenAI Realtime]]
- [[Technologies/ElevenLabs]]
- OpenAI Responses guide: https://platform.openai.com/docs/guides/responses
- ElevenAgents post-call webhooks: https://elevenlabs.io/docs/eleven-agents/workflows/post-call-webhooks

## Related Topics
- [[Workflows/Workflow Index]]
- [[Concepts/Dyadic Animation Data]]
- [[Concepts/Research Source Index]]

## Evidence
- modal_kfldemoapi/app.py
- demo/src/lib/kfldemoApi.ts
- demo/src/App.tsx
- demo/src/InterviewDemoApp.tsx
- supabase-kfl/supabase/migrations/20260430170000_yc_interview_sessions.sql
- supabase-kfl/supabase/migrations/20260512120000_seed_round_pitch_columns.sql
