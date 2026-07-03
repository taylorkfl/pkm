---
type: definition
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/security
---

# Publishable Key

## Objective
Define the browser-safe key used to identify a Keyframe embed configuration without exposing server-only credentials.

## Description
A publishable key is a frontend-safe identifier for an embed config. It can appear in browser code because it is not a general API secret. The backend still gates usage through active config state, origin checks, plan/billing limits, and short-lived runtime credential minting.

## Scope
| Included | Excluded |
| --- | --- |
| `publishableKey` inputs used by `PersonaEmbed` and `<kfl-embed>` | `kfl_sk_...` secret API keys |
| Embed config lookup and browser bootstrap | Provider API keys, service-role keys, signing secrets |
| Public package examples in `js-elements` and `js-react` | Unlimited authorization by itself |

## Use Case
Use this definition when deciding what is safe to put in frontend code or when creating embed/session flows.

## Stage In Lifecycle
Runtime and security.

## Importance
Core. Confusing publishable keys with secret keys can either break embeds or leak backend authority.

## Risks
- A publishable key should not be treated as sufficient authorization by itself.
- It must not be replaced with a secret API key in frontend code.
- Origin checks and active config state are part of the security boundary.

## Input / Output
| Input | Output |
| --- | --- |
| Publishable embed key in browser code | Server-side embed config lookup and short-lived session bootstrap |

## Resources
- [[Concepts/Publishable Key And Secret Key]]
- [[Structure/js-elements/js-elements]]
- [[Structure/js-react/js-react]]
- [[Philosophies/Publishable-Key Embed Model]]

## Related Topics
- [[Concepts/Embed Config]]
- [[Concepts/Voice Agent Details]]
- [[Concepts/Secret Management And Environment Variables]]

## Evidence
- js-elements/README.md
- js-react/README.md
- js-elements/src/PersonaEmbed.ts
- js-react/src/PersonaEmbed.tsx
- modal_kflapi/app.py
- modal_kflapi_tests/test_embed_session.py
