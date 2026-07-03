---
type: workflow
repo: keyframe-live-avatar-demos
status: growing
lifecycle_stage: design
importance: core
last_reviewed: 2026-06-28
aliases:
  - Live Avatar Demo Roadmap
tags:
  - code-pkm
  - repo/keyframe-live-avatar-demos
  - pkm/type/workflow
  - pkm/domain/product
---

# Demo Roadmap

## Objective
Define the staged path from raw demo ideas to customer-ready Keyframe Labs live avatar demonstrations.

## Description
### What
The roadmap organizes three vertical demos around a common delivery pattern: demo story, minimal live prototype, grounded context, post-session output, and production-safety polish.

### Why
Customers learn fastest when they can see a real workflow and then map it to their own environment. A staged roadmap keeps the first version small while preserving a path to production credibility.

### When
Use this roadmap before building screens, scoping engineering work, or deciding which customer demo to show first.

### How
The recommended build order is:

1. Build one shared demo shell and context-ingestion path.
2. Ship the interview preparation demo first because it exercises per-session documents without requiring meeting infrastructure.
3. Add the helpful student aid demo second because it reuses document grounding and tutor personas.
4. Add the Zoom active participant demo third because it introduces meeting admission, invocation, and live group etiquette.
5. Harden cross-demo controls, privacy messaging, evaluation, and observability before customer pilots.

## Scope
| Phase | Goal | Demo Impact | Exit Criteria |
| --- | --- | --- | --- |
| Phase 0 - Narrative kit | Create demo scripts, sample data, personas, and success metrics | All demos | Each demo has a 3-minute story, a 10-minute walkthrough, and sample inputs |
| Phase 1 - Shared prototype shell | Create a single browser app that can launch live avatar sessions and attach context | Interview, student aid | A user can start and stop an avatar, pass demo context, and see state/error feedback |
| Phase 2 - Vertical MVPs | Build the minimum credible flow for each idea | All demos | Each demo has a complete happy path and a visible post-session artifact |
| Phase 3 - Grounding and trust | Add citations, source controls, privacy notices, and fallback behavior | All demos | The avatar can say what it knows, what it used, and when it cannot answer |
| Phase 4 - Customer pilots | Customize demos for selected customer data and workflows | One or two selected demos | Pilot user can bring their own context without engineering intervention |
| Phase 5 - Production packaging | Convert reusable pieces into productized templates or starter kits | All demos | Repeatable deployment story, monitoring, and admin controls exist |

## Use Case
Use this note to decide what to build next and to prevent demo work from turning into three separate products too early.

## Stage In Lifecycle
Design and deployment. The early phases are design-heavy, while later phases turn the demos into repeatable assets.

## Importance
Core. The roadmap is the bridge between compelling ideas and reliable customer demonstrations.

## Risks
- Building the meeting demo first may slow momentum because it depends on meeting-bot logistics, Recall AI, and live meeting etiquette.
- Building all three demos with separate stacks would hide the platform story and increase maintenance cost.
- Without post-session artifacts, customers may remember the avatar quality but miss the production use case.
- Without explicit trust features, grounded-answer demos can overpromise.

## Input / Output
| Input | Output |
| --- | --- |
| Demo idea, target customer, and available sample data | Ranked build plan |
| Keyframe docs and existing platform PKM concepts | Integration choice and implementation path |
| Customer feedback | Updated priorities and production hardening tasks |

## Resources
- [[Demo Tech Stack]]
- [[Shared Supporting Features]]
- [[Interview Preparation Avatar Demo]]
- [[Helpful Student Aid Avatar Demo]]
- [[Zoom Active Participant Avatar Demo]]
- [[Demo Questions]]
- [[Repositories/monorepo/Concepts/Per-Session Context|Per-Session Context]]
- [[Repositories/monorepo/Workflows/Real-Time Persona Session|Real-Time Persona Session]]

## Related Topics
- [[Repositories/monorepo/Philosophies/Layered SDK Abstraction|Layered SDK Abstraction]]
- [[Repositories/monorepo/Philosophies/Real-Time Streaming Pipeline|Real-Time Streaming Pipeline]]
- [[Repositories/monorepo/Concepts/Dynamic Variables|Dynamic Variables]]

## Evidence
- User-provided objective: create customer demonstrations that teach how Keyframe Labs avatars work in production settings.
- Keyframe Labs docs recommend choosing between widget, hosted, and self managed integration approaches: https://docs.keyframelabs.com/guides/integrate/overview
- Existing vault context: `Repositories/monorepo/Concepts/Per-Session Context.md`
- Existing vault context: `Repositories/monorepo/Workflows/Real-Time Persona Session.md`
