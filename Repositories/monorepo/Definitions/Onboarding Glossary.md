---
type: definition
repo: monorepo
status: growing
lifecycle_stage: discovery
importance: core
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/onboarding
---

# Onboarding Glossary

## Objective
Define the repo, web, database, realtime, testing, and operations terms discussed during the onboarding Q&A so each term has a stable Obsidian entry point.

## Description
This note is a compact glossary for terms that came up while reading [[monorepo]], [[Architecture]], and `README.md` conceptually. Use it when a term is too small for its own note or when you need a bridge to deeper notes such as [[Concepts/Local HTTPS And Cloudflared Tunnels]], [[Concepts/Database Migrations And Seeding]], [[Concepts/Testing Taxonomy]], [[Concepts/Auth Redirects And Magic Links]], and [[Concepts/Voice AI Pipeline]].

## Scope
| Term | Short Definition | Deeper Note |
| --- | --- | --- |
| Apps | User-facing runnable programs such as `platform`, `demo`, and `meet-bot`. | [[Structure/Structure Index]] |
| Service | A backend/runtime process or API such as `modal_kflapi`, `modal_kfldemoapi`, `kfl-voice-agent`, or GPU inference workers. | [[Architecture]] |
| Infrastructure / infra | Cloud resources and deployment plumbing such as DNS, tunnels, load balancers, ECS, secrets, and Terraform-managed services. | [[Technologies/Terraform]] |
| ML | Machine-learning systems and workers, especially `ml/windy-inference` and `ml/InfiniteTalk`. | [[Structure/ml/windy-inference/windy-inference]], [[Structure/ml/InfiniteTalk/InfiniteTalk]] |
| Vite React app | A browser frontend built with React and served/bundled by Vite; unlike Streamlit, it is a TypeScript/JavaScript web app rather than a Python-driven UI runtime. | [[Technologies/Vite]], [[Technologies/React]] |
| SDK | A software development kit; in this repo, `js-sdk` is the lower-level browser session package. | [[Definitions/SDK]], [[Structure/js-sdk/js-sdk]] |
| Low-level SDK | A package that exposes transport/session primitives rather than a complete UI. Here it controls connection state, LiveKit room media, audio streams, interruptions, and emotion control. | [[Definitions/PersonaSession]], [[Definitions/createClient]] |
| Elements | Framework-agnostic browser primitives and Web Components from `js-elements`, including `PersonaEmbed`, `PersonaView`, and `<kfl-embed>`. | [[Structure/js-elements/js-elements]], [[Technologies/Web Components]] |
| Framework-agnostic | Usable from many web frameworks because the API is plain browser JavaScript/custom elements rather than React-only. | [[Structure/js-elements/js-elements]] |
| React wrapper | A React-shaped API around lower-level elements behavior, exposing props/components and handling lifecycle cleanup for React apps. | [[Structure/js-react/js-react]], [[Philosophies/Layered SDK Abstraction]] |
| Embeddable client package | A package another app can install or drop into a page to run a Keyframe persona/avatar session. | [[Concepts/Publishable Key And Secret Key]] |
| Publishable key | A browser-safe key that identifies an embed config; it is not a backend secret. | [[Definitions/Publishable Key]] |
| Secret key | A backend-only credential such as service-role keys, API keys, provider keys, webhook secrets, or signing secrets. | [[Concepts/Secret Management And Environment Variables]] |
| CORS | Browser rule that prevents frontend code from calling another origin unless the API opts in with allowed origin headers. | [[Concepts/Auth Redirects And Magic Links]] |
| Origin | Scheme + host + port, such as `http://localhost:5173` or `https://platform.example.com`. | [[Concepts/Auth Redirects And Magic Links]] |
| Webhook callback | Provider-initiated HTTP request into the backend, such as Stripe or ElevenLabs telling the app that an event happened. | [[Concepts/Webhooks]] |
| OAuth redirect | A browser redirect from an auth provider back to the app after login/consent. | [[Concepts/Auth Redirects And Magic Links]] |
| Supabase auth redirect | A Supabase Auth redirect after magic-link, OAuth, email confirmation, or password reset flow. | [[Concepts/Auth Redirects And Magic Links]] |
| Magic link | One-time login link sent by email. | [[Concepts/Auth Redirects And Magic Links]] |
| Auth user | A user managed by Supabase Auth; app tables often reference the auth user id. | [[Concepts/Database Migrations And Seeding]], [[Technologies/Supabase]] |
| Cloudflared tunnel | Cloudflare-managed public HTTPS route into a local service. | [[Technologies/Cloudflared Tunnels]] |
| Ingress | Tunnel routing rules for inbound requests. | [[Concepts/Local HTTPS And Cloudflared Tunnels]] |
| Hostname | Public DNS name used by a tunnel route, such as a dev subdomain. | [[Concepts/Local HTTPS And Cloudflared Tunnels]] |
| Tunnel service | In tunnel config, the local destination URL such as `http://localhost:54321`. | [[Concepts/Local HTTPS And Cloudflared Tunnels]] |
| Migration | Versioned SQL change that evolves schema, functions, policies, and data contracts. | [[Concepts/Database Migrations And Seeding]], [[Technologies/SQL Migrations]] |
| Seed / seeding | Starter data inserted into the database for local/dev/test operation; not cryptographic randomness. | [[Concepts/Database Migrations And Seeding]] |
| Index | Database structure that speeds up lookup paths. | [[Concepts/Database Migrations And Seeding]] |
| Constraint | Database rule such as uniqueness, required values, foreign keys, or valid value checks. | [[Concepts/Database Migrations And Seeding]] |
| Function | Reusable Postgres logic that can compute or mutate state. | [[Concepts/Database Migrations And Seeding]] |
| RPC | A database function exposed for application calls, often used to keep multi-step work atomic. | [[Definitions/Database RPC]] |
| Trigger | Database hook that runs automatically before/after a table change. | [[Concepts/Database Migrations And Seeding]] |
| Race condition | Bug caused by unlucky timing between concurrent operations. | [[Definitions/Race Condition]] |
| Regression test | Test that catches a known bug if it returns. | [[Definitions/Regression Test]], [[Concepts/Testing Taxonomy]] |
| Integration test | Test that checks multiple real components together. | [[Definitions/Integration Test]], [[Concepts/Testing Taxonomy]] |
| STT | Speech-to-text: audio in, transcript text out. | [[Concepts/Voice AI Pipeline]] |
| LLM | Large language model: text/context in, response text out. | [[Concepts/Voice AI Pipeline]] |
| TTS | Text-to-speech: response text in, spoken audio out. | [[Concepts/Voice AI Pipeline]] |
| LiveKit agent | Server-side AI participant that joins a LiveKit room and can listen/publish media/data. | [[Definitions/LiveKit Agent]], [[Concepts/LiveKit Transport]] |
| Video meeting | A realtime meeting/call like Zoom, Google Meet, or Teams. | [[Technologies/Recall.ai]] |
| Recall.ai | Provider used to send a bot into a video meeting with a webpage camera output. | [[Technologies/Recall.ai]], [[Structure/meet-bot/meet-bot]] |
| Secret manager | A production secret storage system such as Modal Secrets, AWS Secrets Manager, Cloudflare Workers secrets, GitHub Actions secrets, or Vault. | [[Technologies/Secret Managers]] |
| Environment variable | Process-level configuration value, usually provided by shell, platform, container, or secret manager. | [[Concepts/Secret Management And Environment Variables]] |

## Use Case
Use this note as the first stop for onboarding terms before opening deeper workflow or structure notes.

## Stage In Lifecycle
Discovery. This note supports reading and onboarding.

## Importance
Core. It compresses the session's shared vocabulary into a navigable repo glossary.

## Risks
- Glossary definitions can become stale if implementation changes; prefer linked notes and evidence paths for durable understanding.
- Some entries are general web/backend concepts, so repo-specific behavior should be checked in linked notes before changing code.

## Input / Output
| Input | Output |
| --- | --- |
| Onboarding term or question | Short definition plus wikilink to deeper repo context |

## Resources
- [[monorepo]]
- [[Architecture]]
- [[Concepts/Concept Index]]
- [[Definitions/Definition Index]]
- [[Technologies/Technology Index]]
- [[Structure/Structure Index]]

## Related Topics
- [[Concepts/Local HTTPS And Cloudflared Tunnels]]
- [[Concepts/Database Migrations And Seeding]]
- [[Concepts/Secret Management And Environment Variables]]
- [[Concepts/Testing Taxonomy]]
- [[Concepts/Auth Redirects And Magic Links]]
- [[Concepts/Voice AI Pipeline]]
- [[Technologies/Recall.ai]]

## Evidence
- README.md
- pnpm-workspace.yaml
- package.json
- supabase-kfl/supabase/config.toml
- supabase-kfl/supabase/package.json
- supabase-kfl/supabase/data/seed-core.ts
- modal_common/main.py
- modal_kflapi/app.py
- modal_kflapi_tests/conftest.py
- kfl-voice-agent/ARCHITECTURE.md
- kfl-voice-agent/DEPLOYMENT.md
- meet-bot/index.html
- External user context: onboarding Q&A in this Codex session on 2026-06-29.
