---
type: structure-note
repo: monorepo
status: growing
lifecycle_stage: design
importance: high
last_reviewed: 2026-06-28
repo_path: modal_common
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/backend
---

# modal_common

## Objective
Explain the shared Python helpers that centralize FastAPI app creation, environment requirements, Supabase clients, LiveKit API setup, and voice-agent credential minting.

## Description
### What
`modal_common` is a small shared backend support package used by Modal-hosted APIs. It contains app/CORS helpers, environment guards, Supabase service/user clients, LiveKit API setup, and voice-agent provider credential helpers.

### Why
The central API and demo API need consistent behavior around required environment variables, allowed origins, Supabase access, and voice-agent token creation. Keeping this shared prevents each service from inventing its own secret-handling patterns.

### When
It is used when creating FastAPI apps, validating environment variables, authenticating dashboard users, creating service-role Supabase clients, creating LiveKit API objects, and minting OpenAI/ElevenLabs/KFL browser-safe credentials.

### How
`main.py` builds CORS-enabled FastAPI apps and Supabase clients from env vars. `agents.py` defines Pydantic discriminated unions for voice-agent config and functions that convert server-held keys into signed URLs or ephemeral tokens.

## Scope
| File | Responsibility |
| --- | --- |
| `main.py` | `require_env_var`, CORS app builder, Supabase admin/user client, LiveKit API helper |
| `agents.py` | Voice-agent config models, ElevenLabs signed URL, OpenAI Realtime client secret, KFL HMAC signed URL |

## Use Case
Use before changing shared backend auth, CORS, environment behavior, Supabase client behavior, or voice-agent credential models.

## Stage In Lifecycle
Design. This package defines shared backend primitives used by runtime services.

## Importance
High. Mistakes here can affect both product and demo APIs or expose credential-handling bugs.

## Risks
Risks include broad blast radius, inconsistent env variable names, accidental browser exposure of secrets, and CORS/auth behavior drifting across services.

## Input / Output
| Input | Output |
| --- | --- |
| Env vars and request headers | Configured FastAPI app, Supabase client, authenticated user context, or LiveKit API helper |
| Stored provider API key and voice-agent config | Browser-safe signed URL or token |

## Resources
- [[Workflows/API Key And Auth Flow]]
- [[Concepts/Voice Agent Details]]
- [[Concepts/Publishable Key And Secret Key]]
- [[Technologies/Pydantic]]
- [[Technologies/FastAPI]]
- [[Technologies/ElevenLabs]]
- OpenAI Realtime guide: https://platform.openai.com/docs/guides/realtime
- ElevenAgents WebSocket docs: https://elevenlabs.io/docs/eleven-agents/libraries/web-sockets

## Related Topics
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Structure/modal_kfldemoapi/modal_kfldemoapi]]
- [[Philosophies/Layered SDK Abstraction]]

## Evidence
- modal_common/main.py
- modal_common/agents.py
- modal_kflapi/app.py
- modal_kfldemoapi/app.py
- modal_kflapi_tests/test_token_minting.py
- modal_kflapi_tests/test_api_auth.py
