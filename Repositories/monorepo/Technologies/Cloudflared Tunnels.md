---
type: technology
repo: monorepo
status: growing
lifecycle_stage: operations
importance: high
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/technology
  - pkm/domain/networking
---

# Cloudflared Tunnels

## Objective
Explain Cloudflared tunnels as the local-to-public HTTPS networking tool referenced by the repo README.

## Description
Cloudflared tunnels let a developer expose a local HTTP service through a Cloudflare-managed public HTTPS hostname. The repo README uses this for environments where local URLs must be HTTPS or externally reachable, especially physical iOS device development.

## Scope
| Feature | Repo Relevance |
| --- | --- |
| `cloudflared login` | Grants the local CLI access to a Cloudflare account/zone |
| Tunnel identity | Stable tunnel credentials created by `cloudflared tunnel create` |
| DNS route | Public hostname routed to the tunnel |
| Ingress config | Hostname-to-local-service routing rules |
| Local connector | Running `cloudflared tunnel run` on the developer machine |

## Use Case
Use this technology when testing local app/API/Supabase behavior from mobile devices, provider webhooks, OAuth redirects, or HTTPS-required browser APIs.

## Stage In Lifecycle
Operations and local development.

## Importance
High. It bridges local developer services into realistic HTTPS environments.

## Risks
- Requires domain/DNS/Cloudflare permission from an admin.
- Exposes local services while running.
- Tunnel credentials and config should be protected.

## Input / Output
| Input | Output |
| --- | --- |
| Cloudflare zone access, tunnel config, local service | Public HTTPS hostname forwarding to local port |

## Resources
- [[Concepts/Local HTTPS And Cloudflared Tunnels]]
- [[Workflows/Cloudflared Tunnel Development]]
- Cloudflare Tunnel docs: https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/

## Related Topics
- [[Concepts/Auth Redirects And Magic Links]]
- [[Concepts/Webhooks]]
- [[Technologies/Cloudflare Workers Wrangler]]

## Evidence
- README.md
- supabase-kfl/supabase/config.toml
- External user context: onboarding Q&A in this Codex session on 2026-06-29.
