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
  - pkm/domain/runtime
---

# Per-Session Context

## Objective
Explain how one live demo or avatar session receives temporary context without changing the permanent persona, agent, or database defaults.

## Description
### What
Per-session context is information attached to a single conversation or runtime session. It can include user-uploaded documents, a generated founder brief, a first message, an interview application summary, draft embed config, reservation/session ids, or a phase nudge sent while the call is already running.

### Why
Demo apps need to feel specific without permanently mutating a persona. The repo keeps durable product configuration in Supabase and injects one-off context through dynamic variables, signed voice-agent URLs, LiveKit room/session metadata, and runtime messages.

### When
Use it when the demo should adapt to one user's uploaded files, one current call, one draft widget, one conversation id, or one local phase of a live session.

### How
The common pattern is:
1. Frontend collects files, URLs, notes, or app state.
2. Demo backend transforms that input into concise context with OpenAI or other helpers.
3. Frontend passes the resulting values into `PersonaEmbed` as dynamic variables or sends contextual updates during the call.
4. Backend/session code records durable identifiers such as `conversation_id`, reservation id, room name, or webhook status in Supabase.

## Scope
| Context Type | Where It Appears | Lifetime |
| --- | --- | --- |
| ElevenLabs dynamic variables | Agent prompt placeholders and first-message context | One conversation |
| `sendContext` updates | Non-interrupting contextual updates over the live agent connection | Current call |
| KFL signed WebSocket claims | Reservation id, session nonce, config source, config id | Token TTL/session |
| LiveKit room credentials | Room name, participant token, agent identity | Realtime session |
| Demo result rows | `yc_interview_sessions` keyed by conversation id | Durable demo result |

## Use Case
Use this note when building a demo that needs uploaded documents, generated openers, contextual steering, draft embed configs, or webhook-linked post-call results.

## Stage In Lifecycle
Runtime. It is created near session start, consumed during the live call, and may leave durable traces in Supabase rows.

## Importance
Core. Per-session context is what makes generic personas become specific demo experiences.

## Risks
- Do not store one user's temporary context in a reusable persona or global agent prompt by accident.
- Do not expose server-held provider keys while minting per-session credentials.
- Keep variable names aligned between ElevenLabs agent prompts and frontend `dynamicVariables`.
- Treat conversation ids, reservation ids, and room names as join keys; changing one side can break webhooks, polling, or cleanup.
- Context can become stale if a demo prepares it once and then the user edits inputs before starting the call.

## Input / Output
| Input | Output |
| --- | --- |
| Uploaded files, URLs, notes, or UI selections | Session-specific brief, opener, or prompt variables |
| Draft embed config id | Runtime voice-agent config for that draft session |
| LiveKit/session request | Room credentials and reservation state |
| ElevenLabs conversation id | Demo result row that webhooks and polling can share |

## Resources
- [[Concepts/Dynamic Variables]]
- [[Concepts/Webhooks]]
- [[Concepts/RPC Over WebRTC]]
- [[Database Tables/YC Interview Sessions Table]]
- [[Database Tables/Reservations Table]]

## Related Topics
- [[Technologies/ElevenLabs]]
- [[Technologies/LiveKit]]
- [[Technologies/Supabase]]
- [[Structure/demo/demo]]
- [[Structure/modal_kfldemoapi/modal_kfldemoapi]]
- [[Structure/kfl-voice-agent/kfl-voice-agent]]

## Evidence
- js-elements/src/PersonaEmbed.ts
- js-elements/src/agents/elevenlabs.ts
- demo/src/App.tsx
- modal_kfldemoapi/app.py
- modal_common/agents.py
- kfl-voice-agent/internal/voiceagent/embed_config.go
- supabase-kfl/supabase/migrations/20260430170000_yc_interview_sessions.sql
