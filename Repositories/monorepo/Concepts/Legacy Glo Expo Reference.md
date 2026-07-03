---
type: concept
repo: monorepo
status: seedling
lifecycle_stage: discovery
importance: medium
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/onboarding
---

# Legacy Glo Expo Reference

## Objective
Clarify the stale-looking README reference to a `glo` Expo app and what appears to exist instead in the current checkout.

## Description
The root README says app-specific instructions may live in a subdirectory such as a `glo Expo app`, and later mentions deploying an Expo web app. In the current checkout, no top-level `glo` directory or Expo package/config was found during the onboarding search. The active frontend app shape is Vite React packages such as `platform` and `demo`, plus embeddable client packages.

## Scope
| Observed | Not Found |
| --- | --- |
| Root README mentions `glo Expo app` and Expo web deployment commands | Top-level `glo` directory |
| Workspace packages include `platform`, `demo`, `js-sdk`, `js-elements`, `js-react`, `meet-bot` | Current `expo` dependency in workspace package manifests |
| `platform` and `demo` deploy with Vite/Wrangler scripts | Current Expo app config such as `app.json` or `app.config.*` |

## Use Case
Use this concept when a README instruction references `glo` or Expo and you need to decide which current app/package likely replaced that old wording.

## Stage In Lifecycle
Discovery.

## Importance
Medium. It prevents stale documentation from sending onboarding work down the wrong path.

## Risks
- The app may exist outside this checkout or on another branch; this note only reflects inspected files.
- The README should be updated if current maintainers confirm `platform`/`demo` replaced the old Expo app.

## Input / Output
| Input | Output |
| --- | --- |
| README wording plus current repo search | Stale-reference hypothesis and current app pointers |

## Resources
- [[Structure/platform/platform]]
- [[Structure/demo/demo]]
- [[Technologies/Vite]]
- [[Technologies/React]]

## Related Topics
- [[Workflows/Local Supabase Development And Seeding]]
- [[Concepts/Local HTTPS And Cloudflared Tunnels]]

## Evidence
- README.md
- pnpm-workspace.yaml
- platform/package.json
- demo/package.json
- package.json
- External user context: onboarding Q&A in this Codex session on 2026-06-29.
