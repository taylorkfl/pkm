---
type: workflow
repo: monorepo
status: growing
lifecycle_stage: operations
importance: high
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/workflow
  - pkm/domain/development
---

# Cloudflared Tunnel Development

## Objective
Document the local Cloudflared tunnel workflow and the logic behind login, tunnel creation, DNS routes, ingress, hostnames, and local services.

## Description
The README workflow is: authenticate to Cloudflare, create a tunnel, create DNS routes for hostnames, define local ingress rules, then run the tunnel. Cloudflare receives traffic for the hostname and sends it through the tunnel process running on the developer machine to a configured local service URL.

## Scope
| Step | Meaning |
| --- | --- |
| `cloudflared login` | Authenticates the local machine to a Cloudflare account/zone |
| `cloudflared tunnel create ...` | Creates a tunnel identity and credentials |
| `cloudflared tunnel route dns ... host.example.com` | Creates a DNS route from hostname to tunnel |
| `config.yaml` `ingress` | Defines which hostnames map to which local services |
| `hostname` | Public DNS name that users/devices/providers call |
| `service` | Local destination such as `http://localhost:54321` |
| `cloudflared tunnel run ...` | Starts the local tunnel connector process |

## Use Case
Use this workflow when a local frontend, API, or Supabase instance needs to be tested from a mobile device or external provider.

## Stage In Lifecycle
Operations and local development.

## Importance
High. Correct tunnel setup avoids confusing local-device and HTTPS failures with app bugs.

## Risks
- Requires company Cloudflare/DNS permission or an admin-created route.
- Public hostnames can reach local services while the tunnel is running.
- Wrong ingress rules can send auth callbacks or API calls to the wrong local port.
- The README contains older domain examples; use current company domain/subdomain guidance from an admin.

## Input / Output
| Input | Output |
| --- | --- |
| Cloudflare login and zone access | Tunnel credentials |
| DNS route and ingress config | Public HTTPS hostname routed to local service |
| Running local service | Externally reachable dev URL |

## Resources
- [[Concepts/Local HTTPS And Cloudflared Tunnels]]
- [[Technologies/Cloudflared Tunnels]]
- [[Concepts/Auth Redirects And Magic Links]]

## Related Topics
- [[Workflows/Local Supabase Development And Seeding]]
- [[Technologies/Cloudflare Workers Wrangler]]
- [[Concepts/Secret Management And Environment Variables]]

## Evidence
- README.md
- supabase-kfl/supabase/config.toml
- External user context: onboarding Q&A in this Codex session on 2026-06-29.
