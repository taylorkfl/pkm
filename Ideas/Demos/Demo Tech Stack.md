---
type: architecture
repo: keyframe-live-avatar-demos
status: growing
lifecycle_stage: design
importance: core
last_reviewed: 2026-06-29
aliases:
  - Live Avatar Demo Stack
  - Demo Architecture
tags:
  - code-pkm
  - repo/keyframe-live-avatar-demos
  - pkm/type/architecture
  - pkm/domain/architecture
---

# Demo Tech Stack

## Objective
Define the shared technical architecture for Keyframe Labs live avatar demos that need per-session context, grounded answers, meeting participation, and production-style controls.

## Description
### What
The demo stack should be a reusable set of layers rather than three one-off applications.

| Layer              | Responsibility                                                                | Likely Choices                                                                              |
| ------------------ | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Demo shell         | Product UI, setup wizard, session controls, transcript, artifacts             | React, TypeScript, Keyframe React or Elements                                               |
| Avatar integration | Render and control the live persona session                                   | Widget, hosted `PersonaEmbed`, or self managed `PersonaView`                                |
| Session backend    | Protect secrets, create sessions, prepare context, route provider credentials | FastAPI or Node API, existing Keyframe session APIs                                         |
| Agent runtime      | STT, LLM, TTS, tool calls, turn-taking, interruptions                         | Keyframe hosted agent path, OpenAI Realtime, ElevenLabs, LiveKit Agents, or KFL voice agent |
| Knowledge layer    | Upload, parse, chunk, retrieve, cite, and summarize customer material         | Postgres/vector store, file storage, code search, connector adapters                        |
| Operations         | Auth, consent, logging, rate limits, evaluation, demo analytics               | Supabase, Postgres RLS, audit logs, dashboards                                              |

### Why
The demos need to show real production patterns: server-side secrets, short-lived sessions, per-session context, observable state, and clear failure behavior. A shared stack makes those lessons repeatable.

### When
Use this note before choosing hosted versus self managed integration for a demo.

### How
Choose the integration lane by how much control the demo needs:

| Demo | Recommended First Lane | Why |
| --- | --- | --- |
| Interview preparation | Self managed for MVP, hosted if using an existing provider agent | Needs file ingestion, custom scoring, per-session context, and post-call artifacts |
| Zoom active participant | Meeting-bot API path with Recall AI | Needs the avatar to join Zoom, Google Meet, or Teams as a participant |
| Helpful student aid | Self managed for adaptive tutoring; hosted for a lightweight first proof | Needs subject-specific context, progress memory, citations, and tutor mode controls |
| Travel consultant | Self managed for itinerary tools and live flight data; hosted for a lightweight planning proof | Needs external travel APIs, user preferences, itinerary state, and disruption handling |
| Developer IDE avatar | VS Code extension as the first client; agent-specific adapters through MCP, OpenCode SDK/plugin, or app contribution | Needs an avatar UI, workspace context, coding-agent orchestration, and permission boundaries |

## Scope
| Included | Notes |
| --- | --- |
| Browser session UI | Start, stop, mute, status, context preview, and transcript panes |
| Backend context preparation | Document parsing, prompt construction, retrieval setup, and provider token minting |
| Keyframe session creation | Use official session and meeting-bot paths rather than handcrafted media plumbing |
| Retrieval and citations | Needed for company knowledge, codebase questions, study material, and interview context |
| External data tools | Needed for flight status, fare options, airport details, loyalty constraints, and itinerary updates |
| Agent adapter layer | Needed for OpenCode, Odysseus, Hermes AI, and future coding-agent integrations |
| Post-session artifacts | Feedback reports, answer citations, study plans, and meeting recap |
| Observability | Session state, latency, error reasons, fallback paths, and demo analytics |

Out of scope for the first prototype:

- Full enterprise SSO.
- Production data-retention automation.
- Full LMS, HRIS, ATS, travel, or code-hosting connector coverage.
- Multi-region deployment unless a specific customer pilot requires it.

## Use Case
Use this note to scope engineering tasks, compare demo approaches, and explain the architecture to technical customers.

## Stage In Lifecycle
Design. It should become more concrete once the first prototype is implemented.

## Importance
Core. The stack is the shared substrate that lets three different demos teach one coherent platform story.

## Risks
- Hosted integration is faster but may hide custom context and persistence work customers want to understand.
- Self managed integration gives maximum control but creates more responsibility for secret handling and UX state.
- Retrieval quality can dominate demo quality; a weak knowledge layer makes the avatar look unreliable even if the avatar layer performs well.
- Meeting-bot demos depend on the behavior of the meeting platform and Recall AI as well as Keyframe.
- Demo code that bypasses production admission, rate limits, or privacy controls can teach the wrong lesson.

## Input / Output
| Input | Output |
| --- | --- |
| Demo type and customer workflow | Integration lane choice |
| User documents, company knowledge, codebase, or study material | Session-specific context and cited retrieval results |
| Flight search, flight status, traveler preferences, and destination constraints | Itinerary options, tradeoffs, and disruption-aware next steps |
| Workspace files, selections, diffs, diagnostics, agent session state, and permissions | Avatar guidance, coding-agent commands, explanations, and review artifacts |
| Persona, voice, LLM, and meeting configuration | Live avatar session or meeting participant |
| Transcript and session events | Evaluation, recap, or learning artifact |

## Resources
- [[Shared Supporting Features]]
- [[Demo Roadmap]]
- [[Repositories/monorepo/Architecture|Monorepo Architecture]]
- [[Repositories/monorepo/Workflows/Real-Time Persona Session|Real-Time Persona Session]]
- [[Repositories/monorepo/Workflows/KFL Voice Agent Session|KFL Voice Agent Session]]
- [[Repositories/monorepo/Concepts/Per-Session Context|Per-Session Context]]
- [[Repositories/monorepo/Technologies/React|React]]
- [[Repositories/monorepo/Technologies/TypeScript|TypeScript]]
- [[Repositories/monorepo/Technologies/LiveKit|LiveKit]]
- [[Repositories/monorepo/Technologies/WebRTC|WebRTC]]
- [[Repositories/monorepo/Technologies/WebSockets|WebSockets]]
- [[Repositories/monorepo/Technologies/Supabase|Supabase]]

External references:
- Integration overview: https://docs.keyframelabs.com/guides/integrate/overview
- Hosted quickstart: https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted
- Self managed quickstart: https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed
- JavaScript SDK reference: https://docs.keyframelabs.com/sdk-reference/javascript
- Create session API: https://docs.keyframelabs.com/api-reference/sessions/create-session
- Create meet bot API: https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot

## Related Topics
- [[Interview Preparation Avatar Demo]]
- [[Zoom Active Participant Avatar Demo]]
- [[Helpful Student Aid Avatar Demo]]
- [[Travel Consultant Avatar Demo]]
- [[Developer IDE Avatar Extension Demo]]
- [[Repositories/monorepo/Concepts/Voice Agent Details|Voice Agent Details]]
- [[Repositories/monorepo/Definitions/PersonaSession|PersonaSession]]

## Evidence
- Keyframe Labs docs describe three integration choices: widget, hosted, and self managed.
- Keyframe Labs JavaScript docs describe low-level SDKs, framework-agnostic Elements, hosted `PersonaEmbed`, self managed `PersonaView`, and React bindings: https://docs.keyframelabs.com/sdk-reference/javascript
- Keyframe Labs create-session API returns WebRTC connection details, participant token, and persona identity: https://docs.keyframelabs.com/api-reference/sessions/create-session
- Keyframe Labs meeting docs use Recall AI to add personas to Zoom, Google Meet, and Microsoft Teams: https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams
- Existing vault context: `Repositories/monorepo/Architecture.md`
- Existing vault context: `Repositories/monorepo/Technologies/LiveKit.md`
- Existing vault context: `Repositories/monorepo/Workflows/KFL Voice Agent Session.md`
