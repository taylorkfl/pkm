---
type: workflow
repo: monorepo
status: growing
lifecycle_stage: operations
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/workflow
  - pkm/domain/security
---

# API Key And Auth Flow

## Objective
Explain how the repo separates dashboard-user auth, server API keys, publishable embed keys, edge secrets, and short-lived provider credentials.

## Description
### What
The repo uses multiple auth layers for different trust boundaries. Dashboard actions use Supabase user JWTs plus org role checks. Server-to-API session calls use API keys. Embed sessions use public publishable keys plus origin restrictions. Edge traffic is protected by an edge secret outside local mode. Provider credentials are stored encrypted and converted to short-lived credentials for browsers.

### Why
Each actor has different privileges. A dashboard admin may create or publish configs; a customer backend may start API sessions; an end-user browser may start only an embed session for a permitted origin; a voice-agent provider key must never be exposed directly.

### When
This logic runs during API-key creation, dashboard embed-config CRUD, public embed session creation, and provider-token minting.

### How
API keys are generated once, hashed for storage, and returned plaintext only at creation. Embed publishable keys are public identifiers but looked up by hash and constrained by active status and allowed origins. Voice-agent keys are encrypted before storage; `/v1/embed/create_session` decrypts only long enough to mint a signed URL or ephemeral token, then discards plaintext references.

## Scope
| Credential | Holder | Stored How | Runtime Use |
| --- | --- | --- | --- |
| Supabase JWT | Dashboard user | Supabase auth | Org membership/role checks |
| API key | Customer server | Hash plus fingerprint/hint | Authenticated API session calls |
| Publishable key | Browser/customer page | Full key plus SHA hash | Active embed config lookup |
| Voice agent key | Server only | Encrypted ciphertext | Mint provider signed URL/client secret |
| KFL WS token | Browser for short time | Not persisted | HMAC-signed WebSocket auth claims |
| Edge secret | Edge/API boundary | Environment | Blocks direct non-local API access |

## Use Case
Use when changing auth, embed security, session creation, provider credential handling, or org permission logic.

## Stage In Lifecycle
Operations and runtime. Auth decisions happen before sensitive state changes and before runtime sessions begin.

## Importance
High. Security boundary mistakes can expose provider credentials or let unauthorized users mutate configs and consume capacity.

## Risks
- Publishable keys are public, so origin validation and active status filtering are essential.
- Returning plaintext API keys after creation would violate one-time-secret expectations.
- Directly exposing OpenAI or ElevenLabs API keys to browser code would expand blast radius.
- Role checks must happen with the user-scoped Supabase client, not only with service-role access.
- Local/dev bypasses must not leak into production edge-secret behavior.

## Input / Output
| Input | Output |
| --- | --- |
| Dashboard user JWT and org id | Authorized or rejected config/API-key operation |
| Customer API key | Org context, plan, API key id, plan limits |
| Publishable key plus Origin | Active embed config or 403/404 |
| Stored encrypted provider key | Short-lived browser-usable signed URL or client secret |

## Resources
- [[Concepts/Publishable Key And Secret Key]]
- [[Concepts/Embed Config]]
- [[Technologies/FastAPI]]
- [[Technologies/Pydantic]]
- [[Technologies/Postgres RLS]]
- [[Technologies/ElevenLabs]]
- OWASP API Security Project: https://owasp.org/API-Security/
- OpenAI Realtime client secrets: https://platform.openai.com/docs/guides/realtime
- ElevenAgents signed URL docs: https://elevenlabs.io/docs/eleven-agents/libraries/web-sockets

## Related Topics
- [[Philosophies/Publishable-Key Embed Model]]
- [[Workflows/Embed Config Builder Lifecycle]]
- [[Workflows/Real-Time Persona Session]]
- [[Structure/modal_kflapi/modal_kflapi]]

## Evidence
- modal_kflapi/app.py
- modal_kflapi/crypto.py
- modal_common/agents.py
- modal_kflapi_tests/test_api_auth.py
- modal_kflapi_tests/test_api_key_creation.py
- modal_kflapi_tests/test_token_minting.py
- modal_kflapi_tests/test_embed_config_rls.py
