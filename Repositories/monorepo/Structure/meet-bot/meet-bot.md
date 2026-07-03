---
type: structure-note
repo: monorepo
repo_path: meet-bot
status: growing
lifecycle_stage: runtime
importance: medium
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/frontend
---

# meet-bot

## Objective
Explain the `meet-bot` static page used as the visual output for a meeting bot.

## Description
`meet-bot` is a small static frontend. Its `index.html` reads an encrypted init token and API base URL from query params, calls `/v1/meet-bot/init`, receives session and voice-agent details, and renders a `PersonaView` from `@keyframelabs/elements`.

## Scope
| Path | Role |
| --- | --- |
| `meet-bot/index.html` | Runtime page for bot camera output |
| `meet-bot/package.json` | Serve/build/deploy scripts |
| `meet-bot/prod-wrangler.jsonc` | Cloudflare Worker deployment config |

## Use Case
Use this note when changing meeting bot output UI, token redemption, or Recall.ai webpage camera behavior.

## Stage In Lifecycle
Runtime and deployment.

## Importance
Medium. It is a narrow but important bridge between [[Technologies/Recall.ai]] and the Keyframe embed/session system.

## Risks
- Missing or expired init token prevents the bot page from joining the session.
- The page imports `@keyframelabs/elements` from `esm.sh`, which is a runtime dependency choice.
- API base URL must be reachable from Recall's browser environment.

## Input / Output
| Input | Output |
| --- | --- |
| `token` and `api` query params | `PersonaView` connected to a Keyframe session |

## Resources
- [[Technologies/Recall.ai]]
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Structure/js-elements/js-elements]]

## Related Topics
- [[Concepts/Voice AI Pipeline]]
- [[Concepts/Publishable Key And Secret Key]]

## Evidence
- meet-bot/index.html
- meet-bot/package.json
- modal_kflapi/app.py
- modal_kflapi_tests/test_meet_bot.py
