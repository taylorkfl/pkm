---
type: concept
repo: keyframe-live-avatar-demos
status: growing
lifecycle_stage: design
importance: high
last_reviewed: 2026-06-28
aliases:
  - Zoom Active Participant Demo
  - Meeting Knowledge Avatar
  - Hey Keyframe Meeting Assistant
tags:
  - code-pkm
  - repo/keyframe-live-avatar-demos
  - pkm/type/concept
  - pkm/domain/product
---

# Zoom Active Participant Avatar Demo

## Objective
Demonstrate a Keyframe Labs avatar that joins a live meeting as an active participant and answers invoked questions from a trusted company knowledge base or codebase.

## Description
### What
The avatar joins a Zoom, Google Meet, or Microsoft Teams call with a camera feed. Participants can invoke it with a phrase such as "Hey Keyframe" or "Hey Atlas." The avatar then answers from approved company sources, cites the source, and returns to passive listening.

### Why
This demo teaches a production meeting pattern: live avatars can be present in shared work contexts, not only embedded in a web page. It also shows how companies can expose internal knowledge through a more natural, face-to-face interface.

### When
Use this demo for customers with heavy meeting workflows, internal knowledge bases, support escalation, engineering onboarding, sales enablement, or codebase Q&A needs.

### How
Recommended flow:

1. Host starts a Zoom, Google Meet, or Teams meeting.
2. Demo operator dispatches a Keyframe persona to the meeting.
3. Avatar joins as a named participant with a clear greeting and consent statement.
4. Participants ask invoked questions such as "Hey Keyframe, what is our SLA for enterprise support?"
5. The backend retrieves from the company knowledge base or codebase.
6. Avatar gives a concise spoken answer and the UI or post-meeting artifact records citations.
7. Meeting ends or the avatar is removed, closing the associated session.

## Scope
| Area | MVP | Improvement |
| --- | --- | --- |
| Meeting platform | One platform path for a live demo | Zoom, Google Meet, and Teams coverage |
| Invocation | Manual phrase detection in transcript or prompt | Wake-word gating, participant name addressing, admin mute/unmute |
| Knowledge source | Curated FAQ or docs folder | Codebase search, Slack/Notion/GitHub connectors, permission-aware retrieval |
| Answer UX | Spoken answer with short citation label | Chat links, confidence indicators, "I don't know" fallback, follow-up memory |
| Admin controls | Start, status, stop | Meeting policy, audit logs, retention, user roles |

## Use Case
Use this demo when a customer wants to imagine an avatar as a shared meeting participant, internal expert, or codebase assistant.

## Stage In Lifecycle
Design. This should likely follow the browser-based demos because it adds meeting logistics and group interaction risks.

## Importance
High. It is a memorable demo because the avatar exists inside the customer's existing collaboration environment.

## Risks
- Meeting consent must be explicit because the avatar hears group audio.
- The invocation model needs to avoid accidental interruptions.
- Company knowledge and codebase retrieval must respect permissions and source freshness.
- The avatar should not answer confidential questions when the meeting audience is not authorized.
- Meeting latency and crosstalk can make the avatar feel awkward if turn-taking is not tuned.
- Codebase answers need citations and uncertainty handling to avoid confident hallucinations.

## Input / Output
| Input | Output |
| --- | --- |
| Meeting URL, persona, voice, model, and greeting | Avatar participant joins the meeting |
| Company knowledge base or codebase index | Grounded spoken answers and citations |
| Participant invocation and question | Retrieved context and concise response |
| Meeting transcript and bot status | Recap, audit trail, and follow-up tasks |

## Resources
- [[Demo Tech Stack]]
- [[Shared Supporting Features]]
- [[Demo Roadmap]]
- [[Demo Questions]]
- [[Repositories/monorepo/Concepts/Voice Agent Details|Voice Agent Details]]
- [[Repositories/monorepo/Concepts/LiveKit Transport|LiveKit Transport]]
- [[Repositories/monorepo/Technologies/WebRTC|WebRTC]]

External references:
- Meetings integration: https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams
- Create meet bot API: https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot
- Get meet bot status API: https://docs.keyframelabs.com/api-reference/meet-bots/get-meet-bot-status
- Stop meet bot API: https://docs.keyframelabs.com/api-reference/meet-bots/stop-meet-bot
- List voices API: https://docs.keyframelabs.com/api-reference/voices/list-voices
- List LLM models API: https://docs.keyframelabs.com/api-reference/llm-models/list-llm-models

## Related Topics
- [[Interview Preparation Avatar Demo]]
- [[Helpful Student Aid Avatar Demo]]
- [[Repositories/monorepo/Technologies/LiveKit|LiveKit]]
- [[Repositories/monorepo/Technologies/WebSockets|WebSockets]]
- [[Repositories/monorepo/Concepts/RPC Over WebRTC|RPC Over WebRTC]]

## Evidence
- User-provided idea: Zoom participants should be able to invoke an avatar like "Hey Alexa" and receive information from a company knowledge base or codebase.
- Keyframe Labs meeting docs describe using Recall AI to add personas to Zoom, Google Meet, and Microsoft Teams with camera feed and meeting audio routed to the avatar: https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams
- Keyframe Labs create meet bot API includes meeting URL, persona, bot name, system prompt, greeting, voice, model, and STT configuration fields: https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot
- Existing vault context: `Repositories/monorepo/Concepts/LiveKit Transport.md`
- Existing vault context: `Repositories/monorepo/Technologies/WebRTC.md`
