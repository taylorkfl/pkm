---
type: concept
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: high
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/auth
---

# Auth Redirects And Magic Links

## Objective
Explain CORS, OAuth redirects, Supabase auth redirects, magic links, and why HTTPS hostnames matter for local and deployed auth flows.

## Description
Browser security and auth providers care about origins and redirect URLs. CORS controls whether browser JavaScript may call a different origin. OAuth and Supabase Auth redirect users back to configured URLs after login, confirmation, password reset, or magic-link flows. For local device testing and provider callbacks, a Cloudflared HTTPS hostname can be easier than `localhost`.

## Scope
| Term | Repo-Relevant Meaning |
| --- | --- |
| CORS | API allowlist of frontend origins, configured by `ALLOWED_ORIGINS` in `modal_common` |
| Origin | Scheme + host + port, such as `http://localhost:5173` |
| OAuth redirect | Auth provider sends browser back to app after login/consent |
| Supabase auth redirect | Supabase Auth sends browser to configured site/redirect URL |
| Magic link | One-time email login link that completes auth when clicked |
| `AUTH_SITE_URL` | Supabase Auth base app URL read by `config.toml` through env |

## Use Case
Use this concept when local auth succeeds in one browser but fails on phone/tunnel, when configuring allowed origins, or when deciding which URLs an auth provider needs.

## Stage In Lifecycle
Runtime and local development.

## Importance
High. Misconfigured CORS and redirect URLs often look like frontend bugs but are security/config problems.

## Risks
- `ALLOWED_ORIGINS` must include the actual frontend origin, including scheme and port/host.
- Redirect URLs must match provider/Supabase config exactly enough for the provider's rules.
- HTTPS may be required for mobile/browser auth features and secure cookies.
- Service-role keys must not be exposed while configuring auth.

## Input / Output
| Input | Output |
| --- | --- |
| Frontend origin and backend CORS config | Browser is allowed or blocked from API calls |
| Auth provider redirect config | User returns to app after login/magic-link flow |

## Resources
- [[Concepts/Local HTTPS And Cloudflared Tunnels]]
- [[Workflows/Local Supabase Development And Seeding]]
- [[Technologies/Supabase]]
- [[Concepts/Secret Management And Environment Variables]]

## Related Topics
- [[Concepts/Publishable Key And Secret Key]]
- [[Workflows/API Key And Auth Flow]]
- [[Structure/modal_common/modal_common]]

## Evidence
- modal_common/main.py
- supabase-kfl/supabase/config.toml
- supabase-kfl/supabase/.env.supabase.local.sample
- platform/src/features/auth/context/AuthProvider.tsx
- platform/src/pages/auth.tsx
- README.md
