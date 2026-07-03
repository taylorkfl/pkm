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

# Dynamic Variables

## Objective
Explain the demo pattern for injecting personalized values into an ElevenLabs agent without editing the agent definition for every session.

## Description
### What
Dynamic variables are key/value values sent to an ElevenLabs conversation at startup. ElevenLabs supports string, number, and boolean values; this repo's current embed prop passes them as a string record. The agent can reference them through prompt placeholders such as `{{founder_pre_read}}`, `{{yc_application_context}}`, `{{brief}}`, or `{{first_message}}`.

### Why
They let one ElevenLabs agent run many personalized demos. The agent dashboard owns the reusable prompt, voice, and model behavior, while the app supplies session-specific data gathered from forms, uploads, URLs, or backend context-prep calls.

### When
Use dynamic variables before the conversation starts when the agent should know facts immediately, choose an opener, or tailor its system prompt for that session.

### How
`PersonaEmbed` accepts a `dynamicVariables` record. For ElevenLabs sessions it passes that record to `ElevenLabsAgent`, which sends `conversation_initiation_client_data` over the ElevenLabs WebSocket with a `dynamic_variables` object. Demo screens prepare values such as YC application context or founder pre-read text before rendering the embed.

## Scope
| Variable Pattern | Local Use |
| --- | --- |
| `yc_application_context` | Injects YC application PDF/context into YC interview demos |
| `founder_pre_read` | Gives an interviewer a concise founder/company brief |
| `brief` | Feeds shared generated context into Lightcone-style demos |
| `first_message` | Lets the app set a session-specific opener |
| Empty-string placeholders | Keeps an agent placeholder defined while intentionally leaving it unused |

## Use Case
Use this note when wiring a new ElevenLabs-backed demo, renaming prompt placeholders, preparing context in `modal_kfldemoapi`, or debugging a generic-sounding agent.

## Stage In Lifecycle
Runtime startup. Values are prepared before the WebSocket begins the conversation.

## Importance
Core for demo applications. This is the simplest path from user input to a personalized live conversation.

## Risks
- Placeholder names must match exactly between ElevenLabs agent prompt configuration and frontend `dynamicVariables`.
- Values are strings, so structured context should be summarized or serialized intentionally.
- Dynamic variables are startup context. Use `sendContext` for later non-interrupting updates during a live call.
- Do not put raw secrets, private service keys, or unnecessary PII into variables sent to a third-party agent.
- If context preparation fails, the fallback value should produce a graceful demo instead of an empty or misleading prompt.

## Input / Output
| Input | Output |
| --- | --- |
| Frontend `dynamicVariables` record | ElevenLabs startup payload with `dynamic_variables` |
| Uploaded document or URL summary | Prompt placeholder value for one conversation |
| Agent prompt placeholder | Personalized agent behavior for the current session |

## Resources
- [[Concepts/Per-Session Context]]
- [[Concepts/Webhooks]]
- [[Technologies/ElevenLabs]]
- [[Workflows/Demo Scoring And Share Flow]]
- ElevenLabs dynamic variables docs: https://elevenlabs.io/docs/eleven-agents/customization/personalization/dynamic-variables

## Related Topics
- [[Structure/demo/demo]]
- [[Structure/js-elements/js-elements]]
- [[Structure/modal_kfldemoapi/modal_kfldemoapi]]
- [[Database Tables/YC Interview Sessions Table]]

## Evidence
- js-elements/src/PersonaEmbed.ts
- js-elements/src/agents/elevenlabs.ts
- demo/src/App.tsx
- modal_kfldemoapi/app.py
- js-react/src/PersonaEmbed.tsx
