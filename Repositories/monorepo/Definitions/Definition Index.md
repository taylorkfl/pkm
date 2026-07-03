---
type: definition
repo: monorepo
status: growing
lifecycle_stage: discovery
importance: high
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/definition
---
# Definition Index

## Objective
The objective of the Definitions layer is to make precise code references, API symbols, protocol nouns, runtime entities, and onboarding terms easy to look up without overloading broader concept notes.

## Description
Definitions answer: what exactly is this thing in the repository, why does it exist, when does code use it, and how does it behave at the boundary where another component depends on it.

They are intentionally narrower than [[Concepts/Concept Index|concept notes]]. A concept explains a reusable idea such as [[Concepts/LiveKit Transport|LiveKit Transport]], [[Concepts/Database Migrations And Seeding|Database Migrations And Seeding]], or [[Concepts/Voice AI Pipeline|Voice AI Pipeline]]. A definition pins down a named repo object or term such as [[Definitions/createClient|createClient()]], [[Definitions/PersonaSession|PersonaSession]], [[Definitions/Database RPC|Database RPC]], or [[Definitions/Onboarding Glossary|Onboarding Glossary]].

## Scope
Included in this index:

| Definition | Kind | Primary Surface |
|---|---|---|
| [[Definitions/Onboarding Glossary|Onboarding Glossary]] | glossary | session Q&A and repo evidence |
| [[Definitions/createClient|createClient()]] | exported factory | `js-sdk/src/index.ts` |
| [[Definitions/PersonaSession|PersonaSession]] | browser session class | `js-sdk/src/PersonaSession.ts` |
| [[Definitions/SDK|SDK]] | package boundary | `js-sdk`, `js-elements`, `js-react` |
| [[Definitions/Publishable Key|Publishable Key]] | browser-safe credential | `js-elements`, `modal_kflapi` |
| [[Definitions/LiveKit Room|LiveKit Room]] | realtime room entity | JS SDK and central API |
| [[Definitions/LiveKit Agent|LiveKit Agent]] | server-side realtime participant | `livekit-plugins-keyframe`, `kfl-voice-agent` |
| [[Definitions/Backend Admission|Backend Admission]] | server-side session gate | `modal_kflapi/app.py` |
| [[Definitions/Live Avatar Session|Live Avatar Session]] | end-to-end runtime session | browser, API, LiveKit, worker |
| [[Definitions/LiveKit Byte Stream|LiveKit Byte Stream]] | audio transport primitive | `lk.audio_stream` |
| [[Definitions/Control Message|Control Message]] | realtime command primitive | `lk.control` |
| [[Definitions/Database RPC|Database RPC]] | Postgres operation boundary | Supabase migrations and API calls |
| [[Definitions/Race Condition|Race Condition]] | concurrency failure mode | reservation, lease, interruption paths |
| [[Definitions/Integration Test|Integration Test]] | test category | `modal_kflapi_tests` |
| [[Definitions/Regression Test|Regression Test]] | test category | bug-prevention tests |

Out of scope:

- Every transitive npm or Python dependency.
- General encyclopedia entries that do not name a repo behavior.
- Glossary-only definitions with no repo evidence.

## Use Case
Use this index when reading code and encountering a named symbol, runtime noun, protocol topic, or API boundary that needs a crisp explanation before following the implementation.

## Stage In Lifecycle
Discovery and runtime. Definitions are most useful while tracing how a request becomes a session and how session objects behave once connected.

## Importance
High. A wiki-style repository PKM becomes much easier to search when exact code references have stable homes and broader architectural notes can link to them.

## Risks
- Definitions can become stale if they paraphrase behavior without evidence paths.
- Too many micro-definitions can make the vault noisy.
- Duplicating concept notes can blur the difference between exact references and mental models.

## Input / Output
| Input | Output |
|---|---|
| A repo symbol, protocol term, class, package, or runtime noun | A canonical note explaining what, why, when, how, risks, inputs, outputs, related topics, and evidence |

## Resources
- [[Structure/js-sdk/js-sdk]]
- [[Concepts/Concept Index]]
- [[Technologies/Technology Index]]
- [[Workflows/Workflow Index]]

## Related Topics
- [[Definitions/Onboarding Glossary|Onboarding Glossary]]
- [[Definitions/Publishable Key|Publishable Key]]
- [[Definitions/Database RPC|Database RPC]]
- [[Definitions/Race Condition|Race Condition]]
- [[Definitions/LiveKit Agent|LiveKit Agent]]
- [[Definitions/Integration Test|Integration Test]]
- [[Definitions/Regression Test|Regression Test]]

## Evidence
- `js-sdk/src/index.ts`
- `js-sdk/src/PersonaSession.ts`
- `js-sdk/src/types.ts`
- `js-elements/README.md`
- `modal_kflapi/app.py`
- `modal_kflapi_tests/conftest.py`
- `supabase-kfl/supabase/migrations`
- `kfl-voice-agent/ARCHITECTURE.md`
