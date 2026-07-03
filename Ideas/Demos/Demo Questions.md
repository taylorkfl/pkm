---
type: questions
repo: keyframe-live-avatar-demos
status: seedling
lifecycle_stage: discovery
importance: medium
last_reviewed: 2026-06-29
aliases:
  - Live Avatar Demo Questions
tags:
  - code-pkm
  - repo/keyframe-live-avatar-demos
  - pkm/type/questions
  - pkm/domain/product
---

# Demo Questions

## Objective
Track unresolved decisions and assumptions for the Keyframe Labs live avatar demo portfolio.

## Description
### What
This note collects open questions that should be answered before the demos are shown to customers or converted into repeatable product templates.

### Why
The demos touch sensitive data, live meetings, educational guidance, and retrieval from private knowledge sources. Unknowns should stay visible rather than buried in implementation.

### When
Review this note during demo planning, after each prototype milestone, and before using customer-provided data.

### How
Answer questions by linking to product decisions, implementation notes, customer feedback, or updated demo notes.

## Scope
### Open
- [ ] Which demo should be the flagship first build: interview preparation, student aid, or meeting participant?
- [ ] Should the first browser demo use hosted `PersonaEmbed`, self managed `PersonaView`, or both as a comparison?
- [ ] What sample job descriptions, interviewer profiles, study materials, and company docs are safe to include in public demos?
- [ ] Do we want demo data to be synthetic, customer-provided, or a mix?
- [ ] What is the minimum acceptable citation UX for grounded answers?
- [ ] Should session transcripts be visible in the live demo, saved only after the call, or optional?
- [ ] What retention policy should be shown for uploaded resumes, meeting audio, and study materials?
- [ ] What LLM and voice defaults should each demo use?
- [ ] Which Keyframe persona should represent each role or subject?
- [ ] What should the avatar say when retrieval confidence is low?
- [ ] What is the allowed behavior for meeting participation: passive listening, invoked answers only, or proactive suggestions?
- [ ] How should meeting participants consent to the avatar's presence?
- [ ] Should the codebase Q&A version search source code, existing PKM notes, or both?
- [ ] Which post-session artifact matters most for each demo: feedback report, meeting recap, study plan, or admin log?
- [ ] What production controls should be visible in the demo versus hidden in admin/debug views?
- [ ] Which flight data provider should power the travel consultant demo, and should the first version use live results or curated sample flight data?
- [ ] Should the travel consultant stop at itinerary planning, or should it hand off to booking, calendar, wallet, and travel-management systems?
- [ ] Which exact Hermes AI repository or product should be evaluated for plugin feasibility?
- [ ] Should the developer IDE avatar ship first as a VS Code extension, an OpenCode plugin, an MCP server, or a standalone client?
- [ ] Does the VS Code webview reliably support the required camera, microphone, autoplay, and WebRTC behavior for a Keyframe persona across VS Code, Cursor, Windsurf, and VSCodium?
- [ ] Should the avatar be allowed to execute coding-agent actions directly, or should it only explain and queue suggested actions for user approval?

### Resolved
- [x] The notes belong in the Obsidian vault's `Ideas/Demos` area.  
  Answer: Use this folder as the demo planning hub and link outward to existing repository PKM notes when platform context matters.  
  Evidence: User-provided knowledge base path and existing `Ideas/Demos/Index.md`.

## Use Case
Use this note as the agenda for product review, prototype kickoff, and customer pilot readiness checks.

## Stage In Lifecycle
Discovery. The questions are meant to turn into decisions as implementation gets closer.

## Importance
Medium. These questions do not block note creation, but several will block a customer-safe pilot.

## Risks
- Skipping privacy and consent questions will make demos harder to show responsibly.
- Skipping integration-lane questions can cause overbuilding.
- Skipping sample-data questions can delay demo recording and sales enablement.

## Input / Output
| Input | Output |
| --- | --- |
| Product uncertainty, customer feedback, implementation discoveries | Open or resolved questions |
| Answered question | Updated demo note, roadmap, or decision |

## Resources
- [[Demo Roadmap]]
- [[Demo Tech Stack]]
- [[Shared Supporting Features]]
- [[Interview Preparation Avatar Demo]]
- [[Zoom Active Participant Avatar Demo]]
- [[Helpful Student Aid Avatar Demo]]
- [[Travel Consultant Avatar Demo]]
- [[Developer IDE Avatar Extension Demo]]

## Related Topics
- [[Repositories/monorepo/Concepts/Per-Session Context|Per-Session Context]]
- [[Repositories/monorepo/Concepts/Production Database Safety|Production Database Safety]]
- [[Repositories/monorepo/Philosophies/Publishable-Key Embed Model|Publishable-Key Embed Model]]

## Evidence
- User-provided demo concepts and purpose on 2026-06-28.
- User-provided travel consultant concept on 2026-06-29.
- User-provided developer IDE and open-source agent plugin concept on 2026-06-29.
- Keyframe Labs docs describe different integration lanes with different levels of control: https://docs.keyframelabs.com/guides/integrate/overview
- Keyframe Labs meeting docs introduce meeting participation via Recall AI: https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams
