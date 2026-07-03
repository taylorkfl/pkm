---
type: workflow
repo: monorepo
status: growing
lifecycle_stage: development
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/workflow
  - pkm/domain/development
---

# Local Development And Docker

## Objective
Summarize which parts of the monorepo can be run locally, what Docker support exists, and where local development still depends on hosted services or GPU infrastructure.

## Description
### What
The repo has local development paths for JavaScript apps/packages, local Supabase, the central FastAPI app, and the Go KFL voice agent. It does not currently include a root Docker Compose stack. Docker support is focused on the KFL voice agent container, while ML/avatar runtime code is oriented around GPU hosts, Modal, or RunPod-style deployment scripts.

### Why
The product spans frontend apps, Supabase, Modal APIs, LiveKit, voice agents, and GPU avatar inference. Some parts are easy to run on a laptop; others need secrets, hosted providers, GPU capacity, or deployment-specific networking.

### When
Use this note when onboarding, testing a demo locally, deciding whether a change needs production/staging infrastructure, or answering "can I spin this up locally?"

### How
Start with the smallest layer that proves the behavior:
1. Run JS apps/packages locally for UI and embed behavior.
2. Run local Supabase for schema, RLS, seed, and API integration work.
3. Run `modal_kflapi` locally when testing API request/response paths with local env.
4. Run `kfl-voice-agent` locally or as a Docker image for first-party WebSocket voice-agent development.
5. Use deployed Modal/GPU/LiveKit paths for full avatar inference and external provider integration unless local secrets and infrastructure are intentionally configured.

## Scope
| Part | Local Path | Docker/Compose Status |
| --- | --- | --- |
| Supabase/Postgres | From `supabase-kfl/supabase`, use local Supabase config/env and seed scripts | Supabase CLI manages its own local containers; no repo compose file found |
| `platform` dashboard | Vite dev server | No Dockerfile found |
| `demo` app | Vite dev server | No Dockerfile found |
| `js-elements` | Vite dev server/build | No Dockerfile found |
| `js-sdk`, `js-react` | TypeScript build/watch/package workflows | No Dockerfile found |
| `modal_kflapi` | Can run a local uvicorn app with env | No Dockerfile found |
| `modal_kfldemoapi` | Source says local dev is intentionally not wired; deploy through Modal | No Dockerfile found |
| `kfl-voice-agent` | Go local run path and Modal/AWS deploy paths | Has `kfl-voice-agent/Dockerfile` |
| `ml/windy-inference` | GPU host scripts for daemon/bridge | No compose file; scripts expect GPU host style setup |
| `ml/InfiniteTalk` | RunPod/SSH/tmux justfile workflow | No compose file found |

## Use Case
Use when selecting a safe development environment, planning a local reproduction, or deciding which subsystem needs staging/production credentials.

## Stage In Lifecycle
Development and operations.

## Importance
High. Correct local setup prevents accidental production database use and reduces time spent chasing infrastructure-only failures.

## Risks
- No root Docker Compose stack was found, so do not expect one command to run the whole product.
- Local env files can accidentally point to production. Check Supabase URLs and keys before running write commands.
- Full avatar sessions need LiveKit, provider credentials, API admission, Supabase state, and GPU workers; a frontend-only run will not prove the entire runtime path.
- `modal_kfldemoapi` is deploy-oriented; local tests may need direct API function testing or a Modal environment.
- ML scripts may assume paths, TLS certs, GPU drivers, tmux, and remote registration behavior.

## Input / Output
| Input | Output |
| --- | --- |
| Local env files and package scripts | Running frontend/API/package process |
| Supabase CLI plus local env | Local Postgres/Auth/Storage-style development services |
| KFL voice-agent Dockerfile | Container image for the Go WebSocket service |
| GPU host scripts | Avatar inference daemon/bridge on a prepared GPU machine |

## Resources
- [[Workflows/Cloudflared Tunnel Development]]
- [[Workflows/Local Supabase Development And Seeding]]
- [[Concepts/Local HTTPS And Cloudflared Tunnels]]
- [[Concepts/Database Migrations And Seeding]]
- [[Concepts/Production Database Safety]]
- [[Concepts/Production Database Safety]]
- [[Technologies/Supabase]]
- [[Structure/kfl-voice-agent/kfl-voice-agent]]
- [[Structure/platform/platform]]
- [[Structure/demo/demo]]
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Structure/modal_kfldemoapi/modal_kfldemoapi]]

## Related Topics
- [[Workflows/Real-Time Persona Session]]
- [[Workflows/KFL Voice Agent Session]]
- [[Concepts/LiveKit Transport]]
- [[Database Tables/Database Table Index]]

## Evidence
- README.md
- package.json
- platform/package.json
- demo/package.json
- js-elements/package.json
- js-sdk/package.json
- js-react/package.json
- supabase-kfl/supabase/config.toml
- kfl-voice-agent/Dockerfile
- kfl-voice-agent/modal_app.py
- kfl-voice-agent/DEPLOYMENT.md
- modal_kflapi/app.py
- modal_kfldemoapi/app.py
- ml/windy-inference/scripts/start.sh
- ml/windy-inference/scripts/stop.sh
- ml/InfiniteTalk/justfile
