---
type: concept
repo: keyframe-live-avatar-demos
status: growing
lifecycle_stage: design
importance: high
last_reviewed: 2026-06-29
aliases:
  - Shared Demo Features
  - Demo Support Features
tags:
  - code-pkm
  - repo/keyframe-live-avatar-demos
  - pkm/type/concept
  - pkm/domain/product
---

# Shared Supporting Features

## Objective
Identify reusable capabilities that make Keyframe Labs avatar demos feel like production workflows instead of isolated proof-of-concept calls.

## Description
### What
The demos should share a foundation of setup, context, trust, and post-session features.

| Feature | Used By | Why It Matters |
| --- | --- | --- |
| Demo launcher | All demos | Lets a customer choose a vertical, persona, data source, and scenario |
| Context setup wizard | All demos | Shows how per-session context enters the live avatar session |
| Persona selector | Student aid, interview, meeting | Demonstrates avatar choice by role, tone, or subject |
| Grounded retrieval | All demos | Lets the avatar answer from documents, knowledge bases, and codebases |
| External tool calls | Travel, meeting, interview, education | Lets the avatar retrieve live facts such as flights, status, schedules, or customer records |
| Agent adapter layer | Developer IDE avatar | Lets the avatar talk to coding agents through OpenCode, MCP, or app-specific APIs without owning the whole agent runtime |
| Citation panel | All demos | Builds trust when the avatar answers factual questions |
| Consent and privacy banner | Interview, meeting, education | Makes recording, sensitive data, and meeting participation explicit |
| Transcript and event timeline | All demos | Helps customers inspect what happened after the call |
| Post-session artifact | Interview, student aid, meeting | Turns a conversation into a useful deliverable |
| Admin/debug panel | All demos | Shows status, model, persona, latency, and fallback reasons |
| Failure fallback | All demos | Keeps demos credible when context is missing or retrieval confidence is low |

### Why
Customers will ask how the demo becomes a production feature. Shared support capabilities provide the answer: security, context control, source grounding, observability, and artifacts.

### When
Add these features as soon as the first demo works end to end. They should be part of the demo language, not late polish.

### How
Build the features as reusable product primitives:

- A scenario config defines persona, voice, provider, prompt template, context inputs, and output artifact.
- A context pipeline prepares uploaded or connected data before session start.
- A session state layer tracks avatar status, agent status, transcript, errors, and post-session output.
- A trust layer displays citations, unknowns, permission boundaries, and data retention notes.

## Scope
| Capability | First Version | Later Version |
| --- | --- | --- |
| Context ingestion | Text files, PDFs, URLs, pasted notes | Connectors for Drive, Notion, Slack, GitHub, LMS, ATS, or CRM |
| Grounding | Chunk and retrieve against demo material | Permission-aware retrieval with source freshness and role filtering |
| Persona choice | Curated stock persona per demo | Branded avatars, customer-selected roles, and persona A/B testing |
| Post-session artifacts | Markdown-style summary and score | Export to CRM, ATS, LMS, Slack, or email |
| Itinerary artifact | Trip summary, selected flights, and next decisions | Calendar, wallet, booking handoff, expense, or travel management integrations |
| Development artifact | Plan, diff summary, review notes, and next commands | Pull request handoff, issue comment, OpenCode session, or local workspace task |
| Observability | Visible session state and errors | Latency dashboard, cost estimates, eval scoring, and replay tools |

## Use Case
Use this note when deciding whether a requested feature belongs in one demo or should become part of the shared demo platform.

## Stage In Lifecycle
Design and operations. These features are not all required for the first working prototype, but they are central to production credibility.

## Importance
High. Shared support features are what help customers imagine using avatars with their own teams, data, and controls.

## Risks
- Too much visible setup can slow down the "wow" moment.
- Too little visible setup can make the demo feel like a black box.
- Citation UX must stay lightweight or it will compete with the avatar.
- Reusing features across demos requires scenario boundaries so education data does not accidentally look like interview data.
- Demo logs and transcripts can contain sensitive material and need explicit handling.

## Input / Output
| Input | Output |
| --- | --- |
| Scenario config and customer-selected data | Prepared session context |
| Avatar and agent events | Transcript, state timeline, and debug information |
| Retrieved passages or code snippets | Cited answer support |
| Conversation transcript | Feedback report, meeting recap, or study plan |

## Resources
- [[Demo Tech Stack]]
- [[Demo Roadmap]]
- [[Interview Preparation Avatar Demo]]
- [[Zoom Active Participant Avatar Demo]]
- [[Helpful Student Aid Avatar Demo]]
- [[Travel Consultant Avatar Demo]]
- [[Developer IDE Avatar Extension Demo]]
- [[Repositories/monorepo/Concepts/Per-Session Context|Per-Session Context]]
- [[Repositories/monorepo/Concepts/Dynamic Variables|Dynamic Variables]]
- [[Repositories/monorepo/Concepts/Voice Agent Details|Voice Agent Details]]

## Related Topics
- [[Repositories/monorepo/Technologies/Supabase|Supabase]]
- [[Repositories/monorepo/Technologies/OpenAI Realtime|OpenAI Realtime]]
- [[Repositories/monorepo/Technologies/ElevenLabs|ElevenLabs]]
- [[Repositories/monorepo/Technologies/LiveKit|LiveKit]]

## Evidence
- User-provided purpose: demos should help customers learn how to use Keyframe Labs avatars in production settings.
- Existing vault context: `Repositories/monorepo/Concepts/Per-Session Context.md`
- Existing vault context: `Repositories/monorepo/Concepts/Dynamic Variables.md`
- Existing vault context: `Repositories/monorepo/Concepts/Voice Agent Details.md`
- Keyframe Labs docs describe hosted and self managed paths that can support different levels of setup control: https://docs.keyframelabs.com/guides/integrate/overview
