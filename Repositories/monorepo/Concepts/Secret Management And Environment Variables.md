---
type: concept
repo: monorepo
status: growing
lifecycle_stage: operations
importance: core
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/security
---

# Secret Management And Environment Variables

## Objective
Explain how env vars and secret managers fit the repo's local, test, deployment, and production workflows.

## Description
Environment variables are process-level configuration. Local workflows often source `.env` files into a shell. Production workflows should usually rely on platform or cloud secret managers so secret values are stored, rotated, audited, and injected without being committed to the repo.

## Scope
| Mechanism | Repo Use |
| --- | --- |
| `.env.supabase.local` | Local Supabase API URL, service key, auth URL, and config values |
| `load_env.sh` | Exports env file entries into the current shell |
| Modal Secrets | Deployed Modal API and voice-agent secret bundles |
| AWS Secrets Manager | Terraform-created containers for AWS ECS voice-agent runtime secrets |
| Cloudflare Worker secrets | Worker deployment secrets outside checked-in config |
| GitHub Actions secrets | CI/CD credentials if workflows are configured externally |
| Vault/Doppler/1Password/Azure/GCP | General secret manager examples; not all are evidenced in this repo |

## Use Case
Use this concept when deciding where to put credentials, whether a value can be committed, and why sourced env vars are different from production secret injection.

## Stage In Lifecycle
Operations and security.

## Importance
Core. The repo touches provider keys, Supabase service-role keys, Stripe webhooks, LiveKit credentials, and signing secrets.

## Risks
- Sourced env vars are temporary and shell-local; they disappear in new terminals and after reboot.
- A gitignored `.env` file is acceptable for local development but not a full production secret strategy.
- Service-role keys and provider secrets must never be exposed to frontend bundles.
- Terraform state should not contain raw secret values; create secret containers separately from values when possible.

## Input / Output
| Input | Output |
| --- | --- |
| Env file or platform secret entry | Process environment available to app/script |
| Secret manager value | Runtime-injected credential without checking value into git |

## Resources
- [[Technologies/Secret Managers]]
- [[Concepts/Publishable Key And Secret Key]]
- [[Workflows/Local Supabase Development And Seeding]]
- [[Structure/modal_common/modal_common]]

## Related Topics
- [[Technologies/Modal]]
- [[Technologies/Terraform]]
- [[Technologies/Supabase]]
- [[Technologies/LiveKit]]

## Evidence
- supabase-kfl/supabase/.env.supabase.local.sample
- supabase-kfl/supabase/load_env.sh
- modal_common/main.py
- modal_kflapi/app.py
- kfl-voice-agent/DEPLOYMENT.md
- terraform/stacks/kfl-aws/voice/main.tf
- platform/prod-wrangler.jsonc
- demo/prod-wrangler.jsonc
