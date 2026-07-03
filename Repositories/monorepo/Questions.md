---
type: questions
repo: monorepo
status: growing
lifecycle_stage: discovery
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/questions
  - pkm/domain/product
---

# Questions

## Objective
Track unresolved repo-learning questions, stale documentation questions, and follow-up reads discovered while building the PKM.

## Description
### What
This is the open-question ledger for the repository PKM. It captures claims that are not yet safe to write as facts, places where product naming or runtime contracts may be stale, and follow-up reads that would make future notes sharper.

### Why
A deep codebase wiki should not pretend every inference is settled. Questions keep the notes honest and make the next reading session efficient.

### When
Add a question when code evidence is incomplete, a comment conflicts with implementation, a provider behavior needs live verification, or a product decision is outside the repo.

### How
Phrase questions as answerable investigations. Include evidence paths, the next read, and whether the question affects product behavior, security, operations, or research.

## Scope
| Question Type | Examples |
| --- | --- |
| Product naming | Old `lingua` language versus Keyframe Labs package/API naming |
| Runtime contract | Which session endpoint is canonical for new integrations? |
| Security | Which browser keys are intentionally public and which are server-only? |
| Research | Which expression labels are product-supported versus model-internal? |
| Testing | Which cross-language protocols need contract tests? |

## Use Case
Use when a note needs an unresolved assumption or when future reading should be made explicit instead of buried in prose.

## Stage In Lifecycle
Discovery and testing.

## Importance
High. Questions prevent accidental certainty and turn uncertainty into a learning backlog.

## Risks
- Questions can become stale if answers are not moved back into concept/workflow notes.
- Product questions, security questions, and research questions should remain distinguishable.
- Some questions cannot be answered from repo evidence alone and need product/team context.

## Input / Output
| Input | Output |
| --- | --- |
| Unclear repo evidence, contradictory source, or uncertain inference | Tracked question with evidence and next-read path |

## Resources
### Open
- [ ] Is the root README intentionally still named `lingua`, or should it be updated to current Keyframe Labs product language?
  Evidence: `README.md`, `package.json`, `js-sdk/README.md`.
  Next read: current product docs or naming decision.

- [ ] Which session path is canonical for new integrations: `/v1/sessions`, `/v1/embed/create_session`, `/v1/sessions/kfl`, or `/v1/sessions/plugins/livekit`?
  Evidence: `modal_kflapi/app.py`, `js-elements/README.md`, `livekit-plugins-keyframe/README.md`.
  Next read: current customer integration guidance.

- [ ] Should `surprised` or `concerned` exist as public SDK emotions, or are `neutral`, `angry`, `sad`, and `happy` the only product-supported labels?
  Evidence: `js-sdk/src/types.ts`, `js-elements/src/agents/types.ts`, `kfl-voice-agent/internal/voiceagent/llm.go`.
  Next read: avatar renderer/control protocol and model label set.

- [ ] Should there be a cross-language protocol contract test for LiveKit topics, audio rates, RPC names, WebSocket event names, and emotion labels?
  Evidence: `js-sdk/src/PersonaSession.ts`, `js-elements/src/agents/kfl.ts`, `kfl-voice-agent/ARCHITECTURE.md`.
  Next read: existing JS/Python/Go tests and protocol fixtures.

- [ ] Which local setup path is expected for a new engineer: pnpm-only, Supabase local, Modal local, or mostly remote services?
  Evidence: `README.md`, `pyproject.toml`, package manifests.
  Next read: developer onboarding docs if available.

- [ ] Are demo model names such as `gpt-5.5`, `gpt-5.4-mini`, and `google/gemini-3.1-flash-lite` placeholders, future targets, or currently provisioned IDs?
  Evidence: `modal_kfldemoapi/app.py`.
  Next read: deployment environment/provider model catalog.

- [ ] Where should calibrated representation data, dyadic data, and richer emotion labels live if research work becomes productized?
  Evidence: `js-elements/src/agents/types.ts`, `kfl-voice-agent/testdata/scenarios/CONTRACT.md`, research concept notes.
  Next read: training/data pipelines and collection specs.

### Resolved
- [x] Does the repo expose a layered public SDK story?
  Answer: Yes: low-level `@keyframelabs/sdk`, framework-agnostic `@keyframelabs/elements`, and drop-in `@keyframelabs/react`.
  Evidence: `js-sdk/README.md`, `js-elements/README.md`, `js-react/README.md`.

## Related Topics
- [[monorepo]]
- [[Architecture]]
- [[Workflows/Test-Driven Reading Path]]
- [[Concepts/Emotion Conditioning]]
- [[Concepts/Research Source Index]]

## Evidence
- README.md
- package.json
- js-sdk/README.md
- js-elements/README.md
- js-react/README.md
- modal_kflapi/app.py
- modal_kfldemoapi/app.py
- supabase-kfl/supabase/migrations
- kfl-voice-agent/ARCHITECTURE.md
