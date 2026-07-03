---
type: concept
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/backend
---

# Webhooks

## Objective
Explain how provider callbacks enter the monorepo and what to protect when adding or changing webhook flows.

## Description
### What
Webhooks are provider-initiated HTTP callbacks into backend routes. In this repo the most important examples are Stripe billing events in the main API and ElevenLabs post-call transcript events in the demo API.

### Why
Some product state is only known after an external system finishes work. Stripe knows when billing events happen. ElevenLabs knows when a conversation transcript is ready. Webhooks let those providers push state back into Supabase-backed workflows.

### When
Use webhooks when a provider is the source of truth for an event that happens after the original request returns, such as a completed checkout, subscription change, invoice event, or finished call transcript.

### How
The backend route verifies the provider signature, parses an event type, checks idempotency or current row state, performs the smallest safe database update, and returns a status code that controls provider retry behavior. Demo scoring records handled failures and returns success to avoid retry loops that would keep charging or reprocessing.

## Scope
| Webhook | Route | Main State |
| --- | --- | --- |
| Stripe billing | `/webhooks/stripe` in `modal_kflapi/app.py` | Subscriptions, plan state, processed event ids |
| ElevenLabs post-call | `/webhooks/elevenlabs/post-call` in `modal_kfldemoapi/app.py` | `yc_interview_sessions` score/status/share fields |

## Use Case
Use this note before adding a provider webhook, changing verification logic, touching Stripe billing state, or changing demo scoring behavior.

## Stage In Lifecycle
Runtime and operations. Webhooks run after external provider events and often mutate durable production data.

## Importance
Core. Webhooks bridge external provider truth into the product database.

## Risks
- Always verify signatures before trusting payloads.
- Make handlers idempotent because providers retry and duplicate delivery is normal.
- Do not process unknown or stale events as if they were current state.
- Return codes matter. A non-2xx response can cause retries; a repeated failure can disable provider delivery.
- Keep raw provider payloads out of logs when they may contain sensitive content.
- Webhook routes often need secret exemptions from edge auth while still doing provider-specific verification.

## Input / Output
| Input | Output |
| --- | --- |
| Provider event plus signature headers | Verified backend event |
| Stripe subscription/invoice event | Updated billing tables and processed-event record |
| ElevenLabs transcript event | Demo score, quote, status, transcript summary, or stored failure |

## Resources
- [[Definitions/Onboarding Glossary]]
- [[Database Tables/Billing Tables]]
- [[Database Tables/Billing Tables]]
- [[Database Tables/YC Interview Sessions Table]]
- [[Concepts/Production Database Safety]]
- [[Workflows/Demo Scoring And Share Flow]]
- ElevenLabs post-call webhooks docs: https://elevenlabs.io/docs/eleven-agents/workflows/post-call-webhooks

## Related Topics
- [[Technologies/ElevenLabs]]
- [[Technologies/Stripe Billing]]
- [[Technologies/Supabase]]
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Structure/modal_kfldemoapi/modal_kfldemoapi]]

## Evidence
- modal_kflapi/app.py
- modal_kfldemoapi/app.py
- supabase-kfl/supabase/migrations/20260210120000_billing.sql
- supabase-kfl/supabase/migrations/20260430170000_yc_interview_sessions.sql
