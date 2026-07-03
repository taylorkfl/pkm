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
  - pkm/domain/security
---

# Secret Managers

## Objective
Define secret managers as a technology category and connect them to the repo's deployment surfaces.

## Description
Secret managers store sensitive runtime values such as API keys, database service keys, signing secrets, webhook secrets, and provider credentials. The repo references several deployment surfaces where secrets are expected to be injected rather than committed.

## Scope
| Secret Manager Type | Examples | Repo Relevance |
| --- | --- | --- |
| Platform secret store | Modal Secrets, Cloudflare Worker secrets, GitHub Actions secrets | Modal apps, Workers, CI/CD |
| Cloud provider secret manager | AWS Secrets Manager, Google Secret Manager, Azure Key Vault | AWS ECS voice-agent path uses AWS secret containers |
| Dedicated secret tooling | HashiCorp Vault, Doppler, 1Password Secrets Automation | Useful pattern, not all directly evidenced |
| Local-only files | `.env` files | Local dev only; must be gitignored and protected |

## Use Case
Use when deciding whether a value belongs in a repo file, a local env file, or a managed secret store.

## Stage In Lifecycle
Operations and deployment.

## Importance
High. Secret handling is a trust boundary across frontend, backend, providers, and infrastructure.

## Risks
- Secret values in Terraform state, git history, logs, or frontend bundles are leaks.
- Local `.env` files do not provide rotation/audit/access control.
- Different deploy targets have different secret injection mechanisms.

## Input / Output
| Input | Output |
| --- | --- |
| Secret value and runtime scope | Securely injected environment/config value at runtime |

## Resources
- [[Concepts/Secret Management And Environment Variables]]
- [[Concepts/Publishable Key And Secret Key]]
- [[Technologies/Modal]]
- [[Technologies/Terraform]]

## Related Topics
- [[Workflows/Local Supabase Development And Seeding]]
- [[Structure/kfl-voice-agent/kfl-voice-agent]]
- [[Structure/modal_kflapi/modal_kflapi]]

## Evidence
- kfl-voice-agent/DEPLOYMENT.md
- modal_kflapi/app.py
- modal_kfldemoapi/app.py
- modal_common/main.py
- terraform/stacks/kfl-aws/voice/main.tf
- supabase-kfl/supabase/.env.supabase.local.sample
