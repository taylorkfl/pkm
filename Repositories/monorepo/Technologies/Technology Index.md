---
type: technology
repo: monorepo
status: growing
lifecycle_stage: discovery
importance: core
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/technology
  - pkm/domain/technology
---

# Technology Index

## Objective
Index first-class technologies used by the monorepo and connect each to its repo role.

## Description
### What
This index lists technologies that materially shape repo behavior.

### Why
Technology notes prevent tag sprawl by making each technology a wiki node with resources, risks, and evidence.

### When
Use before changing dependencies, protocols, provider integrations, frontend packages, or deployment/runtime layers.

### How
Follow the domain clusters and open the technology note linked from a workflow before editing implementation.

## Scope
| Domain | Technologies |
| --- | --- |
| Frontend/build | [[Technologies/TypeScript]], [[Technologies/React]], [[Technologies/Vite]], [[Technologies/pnpm Workspaces]], [[Technologies/Web Components]] |
| API/runtime | [[Technologies/Python]], [[Technologies/FastAPI]], [[Technologies/Pydantic]], [[Technologies/Modal]], [[Technologies/Go]], [[Technologies/WebSockets]] |
| Realtime media | [[Technologies/LiveKit]], [[Technologies/WebRTC]], [[Technologies/DataStreams]] |
| Data/billing/infra | [[Technologies/Supabase]], [[Technologies/Postgres RLS]], [[Technologies/SQL Migrations]], [[Technologies/Stripe Billing]], [[Technologies/Cloudflare Workers Wrangler]], [[Technologies/Cloudflared Tunnels]], [[Technologies/Terraform]], [[Technologies/Cloudflare R2]], [[Technologies/Secret Managers]] |
| Voice/AI | [[Technologies/OpenAI Realtime]], [[Technologies/ElevenLabs]], [[Technologies/Cartesia]], [[Technologies/OpenRouter]], [[Technologies/Soniox]] |
| Meeting/provider integrations | [[Technologies/Recall.ai]] |
| ML/research | [[Technologies/PyTorch]], [[Technologies/GPU Inference]] |

## Use Case
Use as the technology table of contents.

## Stage In Lifecycle
Discovery

## Importance
Core index note.

## Risks
Do not turn every transitive dependency into a note unless it is directly important to repo behavior.

## Input / Output
Not applicable

## Resources
- [[Concepts/Research Source Index]]
- [[Architecture]]
- [[Definitions/Onboarding Glossary]]

## Related Topics
- [[Workflows/Product Learning Path]]
- [[Structure/Structure Index]]
- [[Concepts/Concept Index]]

## Evidence
- All technology notes under Technologies
