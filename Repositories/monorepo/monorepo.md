---
type: repo-map
repo: monorepo
status: growing
lifecycle_stage: discovery
importance: core
last_reviewed: 2026-06-28
aliases:
  - Keyframe Labs Monorepo
  - lingua monorepo
repo_root: .
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/repo-map
  - pkm/domain/product
---

# Monorepo

## Objective
Provide the top-level learning map for the Keyframe Labs monorepo as a connected product, API, realtime media, data, billing, and research system.

## Description
### What
This repository is not just a collection of packages. It is the product system around Keyframe Labs personas: browser SDKs, embeddable elements, dashboard workflows, central APIs, Supabase contracts, realtime media/session infrastructure, a first-party voice-agent runtime, demo surfaces, billing, and research code.

### Why
The codebase is organized around a product promise: a developer or dashboard user can configure a persona, start a realtime session, stream voice-agent output into an avatar, enforce plan/capacity limits, and measure usage. That means the best learning path follows a session from customer-facing entry point to runtime and back to persisted state.

### When
Start here whenever a feature request feels scattered across frontend, API, database, and runtime files. The repository often hides the actual behavior in contracts between layers: API response shapes, SQL RPCs, LiveKit topics, WebSocket events, and tests.

### How
Read the repo by following four loops:
1. Product loop: dashboard or SDK call -> central API -> Supabase -> session response.
2. Realtime loop: browser audio -> voice agent -> PCM output -> Persona session -> LiveKit avatar media.
3. Operations loop: reservation -> dispatch -> ready/in-use/released -> reapers and meter reporting.
4. Research loop: representation assumptions -> model/runtime interfaces -> tests or experiments.

## Scope
| Area | What It Owns | Deep-Dive Note |
| --- | --- | --- |
| Public packages | Customer SDK, Web Components, React wrapper | [[Structure/js-sdk/js-sdk]], [[Structure/js-elements/js-elements]], [[Structure/js-react/js-react]] |
| Product apps | Dashboard, widget builder, billing UI, demo UI | [[Structure/platform/platform]], [[Structure/demo/demo]] |
| Central API | API keys, sessions, embed configs, billing, Stripe webhooks, reservations | [[Structure/modal_kflapi/modal_kflapi]] |
| Supabase contracts | Tables, RLS, atomic RPCs, seed data, migrations | [[Structure/supabase-kfl/supabase/supabase]], [[Database Tables/Database Table Index]] |
| Realtime runtime | LiveKit media transport plus KFL voice-agent WebSocket runtime | [[Concepts/LiveKit Transport]], [[Structure/kfl-voice-agent/kfl-voice-agent]] |
| Meeting bot | Recall.ai webpage output for video meetings | [[Structure/meet-bot/meet-bot]], [[Technologies/Recall.ai]] |
| Infrastructure | Terraform, Cloudflare/DNS/tunnels, secret containers, deployment plumbing | [[Structure/terraform/terraform]], [[Technologies/Terraform]], [[Technologies/Cloudflared Tunnels]] |
| ML runtime | Realtime and queued GPU video generation systems | [[Structure/ml/windy-inference/windy-inference]], [[Structure/ml/InfiniteTalk/InfiniteTalk]] |
| Media tooling | Remotion compositions for video/share-card rendering | [[Structure/remotion-mono/remotion-mono]] |
| Tests | Behavioral documentation for auth, limits, drafts, sessions, interruptions | [[Workflows/Test-Driven Reading Path]], [[Structure/modal_kflapi_tests/modal_kflapi_tests]] |
| Tests | Behavioral documentation for auth, limits, drafts, sessions, interruptions | [[Workflows/Test-Driven Reading Path]] |
| Research | Representation split, emotion conditioning, dyadic data assumptions | [[Workflows/S1 S2 Research Plan]], [[Concepts/Representation Bottleneck]] |

## Use Case
Use this note when orienting yourself, planning a code-reading session, or deciding which subsystem owns a product behavior.

## Stage In Lifecycle
Discovery. This is the map-of-maps and should remain broader than any subsystem-specific note.

## Importance
Core. Without a strong root map, the monorepo reads like unrelated apps rather than one session-oriented product system.

## Risks
- Old naming and newer Keyframe Labs naming coexist, so search by product concepts as well as package names.
- Some paths require external services and secrets, making tests and migrations more reliable than local manual runs for understanding intent.
- The hardest bugs are usually boundary bugs: topic mismatch, token shape mismatch, plan-limit mismatch, stale draft state, or reservation cleanup failure.

## Input / Output
| Input | Output |
| --- | --- |
| Learning goal, feature request, bug report, or unknown filename | A path through structure, workflow, concept, technology, and evidence notes |
| Repo evidence such as entry points, migrations, tests, package manifests | A connected explanation of what the code does, why it does it, when it runs, and how data moves |

## Resources
- [[Definitions/Onboarding Glossary]]
- [[Concepts/Local HTTPS And Cloudflared Tunnels]]
- [[Workflows/Local Supabase Development And Seeding]]
- [[Concepts/Database Migrations And Seeding]]
- [[Architecture]]
- [[Architecture]]
- [[Workflows/Product Learning Path]]
- [[Workflows/Workflow Index]]
- [[Structure/Structure Index]]
- [[Concepts/Concept Index]]
- [[Technologies/Technology Index]]
- [[Philosophies/Philosophy Index]]
- [[Database Tables/Database Table Index]]
- [[Workflows/Local Development And Docker]]
- [[Concepts/Research Source Index]]
- [[Questions]]

## Related Topics
- [[Workflows/Real-Time Persona Session]]
- [[Workflows/Embed Config Builder Lifecycle]]
- [[Workflows/Billing Usage And Session Limits]]
- [[Workflows/KFL Voice Agent Session]]
- [[Concepts/Legacy Glo Expo Reference]]
- [[Concepts/Secret Management And Environment Variables]]
- [[Concepts/Production Database Safety]]
- [[Concepts/Production Database Safety]]
- [[Philosophies/Layered SDK Abstraction]]
- [[Philosophies/Reservation-Based GPU Capacity]]

## Evidence
- README.md
- package.json
- pnpm-workspace.yaml
- pyproject.toml
- js-sdk/src/PersonaSession.ts
- js-elements/src/PersonaEmbed.ts
- platform/src/features/agents/useWidgetBuilderStore.ts
- modal_kflapi/app.py
- supabase-kfl/supabase/migrations
- kfl-voice-agent/ARCHITECTURE.md
- kfl-voice-agent/internal/voiceagent/session_core.go
