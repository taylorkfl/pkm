---
type: concept
repo: monorepo
status: growing
lifecycle_stage: operations
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/security
---

# Publishable Key And Secret Key

## Objective
Distinguish browser-safe public identifiers from server-only secrets and explain how the repo converts secrets into short-lived runtime credentials.

## Description
### What
Publishable keys identify active embed configs and are safe to put in browser code. Secret keys include API keys, provider keys, service-role keys, node keys, and HMAC secrets. The repo stores or handles these differently based on who is allowed to possess them.

### Why
Public embeds need a lightweight browser bootstrap path, but provider credentials and service-role access must remain server-side. The publishable-key model lets a browser request a session without gaining general API authority.

### When
This distinction matters during API-key creation, embed config publishing, embed session startup, OpenAI/ElevenLabs/KFL credential minting, and KFL WebSocket auth.

### How
API keys are hashed and returned once. Publishable keys are generated and stored, but lookup uses `pk_hash_b64`. Provider keys are encrypted before storage and decrypted only long enough to mint a signed URL or ephemeral token. KFL signed URLs include an HMAC token with reservation and optional config claims.

## Scope
| Key Type | Browser Safe? | Repo Handling |
| --- | --- | --- |
| Publishable embed key | Yes | Public value, hash lookup, active status/origin gates |
| Customer API key | No | Hash at rest, returned once, maps to org/plan/API key id |
| Voice provider API key | No | Encrypted at rest, decrypted only to mint provider credential |
| OpenAI client secret | Short-lived | Minted server-side for Realtime browser connection |
| ElevenLabs signed URL | Short-lived | Minted server-side from provider API key and agent id |
| KFL signed URL token | Short-lived | HMAC signed, reservation-bound, optional config source claims |

## Use Case
Use when auditing credential flow or changing any browser/session/bootstrap contract.

## Stage In Lifecycle
Operations and runtime. Keys define what can cross trust boundaries.

## Importance
Core. A wrong key boundary can expose secrets or allow unauthorized usage.

## Risks
- Treating publishable keys as authorization by themselves is unsafe; origin and active config checks are required.
- Long-lived provider keys in browser code would leak customer/provider access.
- Reusing KFL signed URLs beyond TTL would weaken reservation binding.
- Hash and ciphertext fields must not be confused: one supports lookup, the other protects a secret.

## Input / Output
| Input | Output |
| --- | --- |
| API key plaintext at creation | Stored hash/fingerprint/hint plus one-time plaintext response |
| Publishable key | Config hash lookup and active config response |
| Provider secret | Ephemeral client token, signed URL, or KFL auth URL |

## Resources
- [[Definitions/Publishable Key]]
- [[Concepts/Secret Management And Environment Variables]]
- [[Workflows/API Key And Auth Flow]]
- [[Workflows/API Key And Auth Flow]]
- [[Concepts/Embed Config]]
- [[Concepts/Voice Agent Details]]
- OpenAI Realtime docs: https://platform.openai.com/docs/guides/realtime
- ElevenAgents WebSocket docs: https://elevenlabs.io/docs/eleven-agents/libraries/web-sockets

## Related Topics
- [[Philosophies/Publishable-Key Embed Model]]
- [[Technologies/OpenAI Realtime]]
- [[Technologies/ElevenLabs]]
- [[Technologies/Postgres RLS]]

## Evidence
- modal_kflapi/app.py
- modal_kflapi/crypto.py
- modal_common/agents.py
- kfl-voice-agent/internal/voiceagent/embed_config.go
- modal_kflapi_tests/test_api_key_creation.py
- modal_kflapi_tests/test_token_minting.py
- modal_kflapi_tests/test_embed_session.py
