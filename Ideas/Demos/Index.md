---
type: repo-map
repo: keyframe-live-avatar-demos
status: growing
lifecycle_stage: design
importance: core
last_reviewed: 2026-06-29
aliases:
  - Keyframe Live Avatar Demos
  - Live Avatar Demo Portfolio
tags:
  - code-pkm
  - repo/keyframe-live-avatar-demos
  - pkm/type/repo-map
  - pkm/domain/product
---

# Keyframe Live Avatar Demo Portfolio

## Objective
Create a connected planning hub for customer-facing demonstrations that teach how Keyframe Labs live avatars can be used in realistic production settings.

## Description
### What
This demo portfolio turns five early ideas into structured product demonstrations:

| Demo | Customer Lesson | Primary Note |
| --- | --- | --- |
| Interview preparation | Per-session documents can make a live avatar feel specific to a role, candidate, and interviewer. | [[Interview Preparation Avatar Demo]] |
| Zoom active participant | A live avatar can join a meeting, listen for an invocation, and answer from trusted company knowledge. | [[Zoom Active Participant Avatar Demo]] |
| Helpful student aid | Subject-specific tutors can guide students through study material with expert persona framing. | [[Helpful Student Aid Avatar Demo]] |
| Travel consultant | Live travel data and user preferences can become a guided itinerary planning conversation. | [[Travel Consultant Avatar Demo]] |
| Developer IDE avatar | A live avatar can sit beside a coding agent in VS Code, explain work, and route tasks through agent integrations. | [[Developer IDE Avatar Extension Demo]] |

### Why
The purpose is not only to impress customers with a realistic avatar. Each demo should teach a deployable pattern: how to choose an integration lane, inject context, ground answers, handle privacy, and expose controls a production user would expect.

### When
Use this hub when choosing which demo to build first, shaping customer narratives, preparing demo data, or deciding which supporting features should be shared across demos.

### How
Read in this order:

1. [[Demo Roadmap]] for staged delivery and priorities.
2. [[Demo Tech Stack]] for architecture and integration choices.
3. [[Shared Supporting Features]] for capabilities reused across demos.
4. The five demo notes for vertical-specific product details.
5. [[Demo Questions]] for assumptions to resolve before customer pilots.

## Scope
| Area | In Scope | Out Of Scope For First Pass |
| --- | --- | --- |
| Customer education | Showing how avatars apply to real business and learning workflows | A generic marketing page |
| Demo design | Flow, inputs, outputs, roadmap, risks, and production considerations | Full implementation specs for every screen |
| Technical framing | Keyframe integration lane, meeting bot path, session context, RAG, and observability | Rewriting the existing monorepo PKM |
| Production readiness | Privacy, permissions, grounding, latency, fallback behavior, and post-session artifacts | Enterprise deployment certification |

## Use Case
Use these notes to plan customer demos, build prototype tickets, brief sales or success teams, and explain why live avatars are useful beyond a novelty interaction.

## Stage In Lifecycle
Design. The notes are ready to guide prototype work and should evolve as demo implementations become real.

## Importance
Core. A strong demo portfolio makes Keyframe Labs easier to understand for customers who need to see production patterns, not just avatar quality.

## Risks
- The demos can become too broad if each one tries to prove every platform feature.
- Grounded knowledge workflows require permissioning, source attribution, and clear fallback behavior.
- Meeting demos need explicit consent and a careful interruption model.
- Interview and education demos can touch sensitive personal or educational data, so privacy needs to be visible from the first prototype.
- Customers may confuse a polished demo with a product-ready packaged vertical unless the roadmap and boundaries are explicit.

## Input / Output
| Input | Output |
| --- | --- |
| User-provided demo ideas, Keyframe Labs docs, and existing monorepo PKM notes | Connected Obsidian notes for demo planning |
| Customer segment or meeting context | Recommended demo path and feature set |
| Prototype feedback | Updated roadmap, risks, and questions |

## Resources
- [[Demo Roadmap]]
- [[Demo Tech Stack]]
- [[Shared Supporting Features]]
- [[Interview Preparation Avatar Demo]]
- [[Zoom Active Participant Avatar Demo]]
- [[Helpful Student Aid Avatar Demo]]
- [[Travel Consultant Avatar Demo]]
- [[Developer IDE Avatar Extension Demo]]
- [[Demo Questions]]
- [[Repositories/monorepo/Architecture|Monorepo Architecture]]
- [[Repositories/monorepo/Concepts/Per-Session Context|Per-Session Context]]
- [[Repositories/monorepo/Workflows/Real-Time Persona Session|Real-Time Persona Session]]

External references:
- Keyframe Labs documentation: https://docs.keyframelabs.com/
- Integration overview: https://docs.keyframelabs.com/guides/integrate/overview
- Meetings integration: https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams
- JavaScript SDK reference: https://docs.keyframelabs.com/sdk-reference/javascript

## Related Topics
- [[Repositories/monorepo/Concepts/Persona|Persona]]
- [[Repositories/monorepo/Concepts/Voice Agent Details|Voice Agent Details]]
- [[Repositories/monorepo/Technologies/LiveKit|LiveKit]]
- [[Repositories/monorepo/Technologies/WebRTC|WebRTC]]
- [[Repositories/monorepo/Technologies/WebSockets|WebSockets]]

## Evidence
- User-provided brief on 2026-06-28:
  - Interview preparation should use the job description and interviewer resume.
  - Zoom active participant should answer invoked questions from a company knowledge base or codebase.
  - Helpful student aid should support subject-specific tutor avatars.
- User-provided travel consultant idea on 2026-06-29: an AI avatar should access flight information and walk the user through an itinerary.
- User-provided developer IDE idea on 2026-06-29: investigate a VS Code extension and plugin paths for open source agents such as Hermes AI, OpenCode, and Odysseus.
- Keyframe Labs docs describe widget, hosted, and self managed integration lanes: https://docs.keyframelabs.com/guides/integrate/overview
- Keyframe Labs docs describe meeting personas for Zoom, Google Meet, and Microsoft Teams: https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams
- Existing vault context: `Repositories/monorepo/Architecture.md`
- Existing vault context: `Repositories/monorepo/Concepts/Per-Session Context.md`
- Existing vault context: `Repositories/monorepo/Workflows/Real-Time Persona Session.md`
