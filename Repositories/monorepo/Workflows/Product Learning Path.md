---
type: workflow
repo: monorepo
status: growing
lifecycle_stage: discovery
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/workflow
  - pkm/domain/product
---

# Product Learning Path

## Objective
Provide a staged, code-evidence-backed reading route that teaches what the repo does, why each layer exists, when behavior runs, and how data moves.

## Description
### What
This is the recommended learning sequence for the repo. It starts from the user-visible product flow, then follows the same behavior down through SDK, API, database, runtime, tests, and research notes.

### Why
Monorepos are easy to learn poorly by reading folders alphabetically. This repo is easier to understand by following state transitions: a persona is configured, a session is admitted, capacity is reserved, a realtime channel is opened, audio is streamed, and usage is reported.

### When
Use this path when onboarding, preparing to modify a feature, or recovering context after getting lost in individual files.

### How
Read in this order:
1. [[monorepo]] for the map.
2. [[Architecture]] for boundaries.
3. [[Workflows/Embed Config Builder Lifecycle]] for how deployable widget configs are created.
4. [[Workflows/Real-Time Persona Session]] for how an end-user session starts.
5. [[Workflows/KFL Voice Agent Session]] for the first-party STT/LLM/TTS turn loop.
6. [[Workflows/Billing Usage And Session Limits]] for the admission and metering loop.
7. [[Workflows/Test-Driven Reading Path]] for failure-mode proof.
8. [[Workflows/S1 S2 Research Plan]] for the research assumptions behind representation and emotion notes.

## Scope
| Stage | Question To Answer | Notes |
| --- | --- | --- |
| Product surface | What can the user do? | [[Structure/platform/platform]], [[Structure/demo/demo]], [[Structure/js-elements/js-elements]] |
| API contract | What must the server validate? | [[Structure/modal_kflapi/modal_kflapi]], [[Concepts/Publishable Key And Secret Key]] |
| Database contract | What is persisted and protected? | [[Structure/supabase-kfl/supabase/supabase]], [[Technologies/Postgres RLS]] |
| Runtime contract | What streams during a live session? | [[Concepts/LiveKit Transport]], [[Workflows/KFL Voice Agent Session]] |
| Operations | What releases capacity and reports usage? | [[Concepts/Reservation And Node Capacity]], [[Concepts/Billing Credits And Plans]] |
| Research | What model assumptions shape product behavior? | [[Concepts/Representation Bottleneck]], [[Concepts/Emotion Conditioning]] |

## Use Case
Use as a checklist for learning or as the first note to open when a new bug report spans multiple systems.

## Stage In Lifecycle
Discovery. It is a deliberate reading workflow, not a runtime workflow.

## Importance
Core. It reduces cognitive load by turning the repo into a sequence of understandable product questions.

## Risks
- Skipping the tests hides why edge cases exist.
- Skipping migrations hides the true source of concurrency and authorization rules.
- Reading the voice-agent runtime before the embed/session contracts can make the system feel more complex than it is.

## Input / Output
| Input | Output |
| --- | --- |
| A learning objective or feature area | A staged reading plan with linked notes and repo evidence |
| New evidence discovered while reading | Updates to concept/workflow notes and open questions |

## Resources
- [[monorepo]]
- [[Architecture]]
- [[Workflows/Workflow Index]]
- [[Concepts/Research Source Index]]
- [[Questions]]

## Related Topics
- [[Philosophies/Test-Driven Code Reading]]
- [[Philosophies/Layered SDK Abstraction]]
- [[Philosophies/Real-Time Streaming Pipeline]]
- [[Structure/Structure Index]]

## Evidence
- README.md
- pnpm-workspace.yaml
- js-sdk/src/PersonaSession.ts
- js-elements/src/PersonaEmbed.ts
- platform/src/features/agents/useWidgetBuilderStore.ts
- modal_kflapi/app.py
- supabase-kfl/supabase/migrations
- kfl-voice-agent/ARCHITECTURE.md
- modal_kflapi_tests
- kfl-voice-agent/internal/voiceagent/*_test.go
