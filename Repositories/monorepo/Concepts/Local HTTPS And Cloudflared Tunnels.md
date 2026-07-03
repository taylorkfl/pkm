---
type: concept
repo: monorepo
status: growing
lifecycle_stage: operations
importance: high
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/development
---

# Local HTTPS And Cloudflared Tunnels

## Objective
Explain why local development sometimes needs public HTTPS URLs and how Cloudflared tunnels map those URLs to local services.

## Description
Cloudflared tunnels are useful when a local service must be reachable from outside the laptop or must appear as HTTPS. The README calls this out for local physical iOS device development because a phone cannot use the laptop's `localhost`, and many browser/mobile capabilities require a secure context.

## Scope
| Need | Why A Tunnel Helps |
| --- | --- |
| Physical iOS testing | The phone can reach a public HTTPS hostname instead of laptop `localhost` |
| Microphone/camera/WebRTC | Browsers often require secure origins for sensitive APIs |
| OAuth and Supabase auth redirects | Providers need a reachable redirect URL |
| Webhook callbacks | Providers need to call back into a URL reachable from the internet |
| Secure cookies/service workers | Many browser security features require HTTPS |
| Local Supabase/API/frontend demos | One tunnel can route several hostnames to different local ports |

## Use Case
Use this concept when testing a local app from a phone, configuring redirect URLs, or exposing local Supabase/API/frontend services temporarily.

## Stage In Lifecycle
Operations and local development.

## Importance
High. It explains why `http://localhost` is not enough for some development workflows.

## Risks
- Public tunnels expose local services while running; stop them when not actively developing.
- Do not expose admin or secret-bearing services unless access controls are understood.
- Using the company domain requires DNS/Cloudflare permission or a delegated subdomain.
- Tunnel credentials should not be committed.

## Input / Output
| Input | Output |
| --- | --- |
| Cloudflare account access, tunnel credentials, DNS route, local service port | Public HTTPS hostname routed to local service |

## Resources
- [[Technologies/Cloudflared Tunnels]]
- [[Workflows/Cloudflared Tunnel Development]]
- [[Workflows/Local Supabase Development And Seeding]]
- [[Concepts/Auth Redirects And Magic Links]]
- [[Concepts/Secret Management And Environment Variables]]

## Related Topics
- [[Technologies/Cloudflare Workers Wrangler]]
- [[Technologies/WebRTC]]
- [[Technologies/Supabase]]
- [[Concepts/Webhooks]]

## Evidence
- README.md
- supabase-kfl/supabase/config.toml
- modal_common/main.py
- External user context: onboarding Q&A in this Codex session on 2026-06-29.
