---
type: structure-note
repo: monorepo
repo_path: terraform
status: growing
lifecycle_stage: deployment
importance: high
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/infra
---

# terraform

## Objective
Explain the infrastructure-as-code area of the monorepo.

## Description
`terraform` contains cloud infrastructure definitions. It manages or supports resources such as Cloudflare zone records, AWS voice-agent infrastructure, shared Terraform backends, and reusable modules. Infra means the cloud/network/runtime resources that applications depend on but do not implement directly.

## Scope
| Path | Role |
| --- | --- |
| `terraform/stacks/kfl-zone` | Cloudflare/DNS-style zone stack |
| `terraform/stacks/kfl-aws/voice` | AWS ECS/EC2 voice-agent runtime infrastructure |
| `terraform/stacks/hue-zone` | Separate zone stack |
| `terraform/env/shared` | Shared backend config files |
| `terraform/modules/secrets` | Terraform module for secret containers |

## Use Case
Use this note when changing cloud resources, DNS, voice-agent AWS deployment, or secret container definitions.

## Stage In Lifecycle
Deployment and operations.

## Importance
High. Terraform changes affect shared/cloud infrastructure, not just local code.

## Risks
- Terraform state can leak metadata and should not contain raw secret values.
- DNS/certificate changes may require multi-step applies.
- Mutable image tags are discouraged; deployment notes prefer pinned image digests.

## Input / Output
| Input | Output |
| --- | --- |
| Terraform variables, backend config, stack files | Cloud resources and outputs |

## Resources
- [[Technologies/Terraform]]
- [[Technologies/Secret Managers]]
- [[Concepts/Secret Management And Environment Variables]]
- [[Structure/kfl-voice-agent/kfl-voice-agent]]

## Related Topics
- [[Concepts/Local HTTPS And Cloudflared Tunnels]]
- [[Technologies/Cloudflared Tunnels]]

## Evidence
- README.md
- kfl-voice-agent/DEPLOYMENT.md
- terraform/stacks/kfl-zone/main.tf
- terraform/stacks/kfl-aws/voice/main.tf
- terraform/modules/secrets/main.tf
- terraform/env/shared/kfl-aws-voice.backend.hcl
