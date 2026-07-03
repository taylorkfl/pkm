---
type: structure-note
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/structure
---

# modal_kflapi

## Objective
Explain the `modal_kflapi` repo area as part of the larger realtime persona product architecture.

## Description
### What
Central FastAPI service for product API keys, sessions, embed configs, reservations, billing, and webhooks.

### Why
The API is the trust and admission boundary. It validates callers, resolves plan/org/persona context, handles secrets, delegates atomic state to Supabase, and dispatches runtime work to nodes.

### When
Read this note when a change touches this folder or when a workflow note points here as evidence.

### How
Request -> auth/limits/billing/persona -> reservation -> LiveKit room/token creation -> node dispatch -> cleanup/reporting.

## Scope
| Reading Layer | What To Look For |
| --- | --- |
| Entry points | Which file receives user/API/runtime traffic first |
| Contracts | Request/response types, events, SQL schema, config structs |
| State transitions | How objects move between draft/active, reserved/ready/released, listening/speaking |
| Tests | Negative paths and product rules |

## Use Case
Use as the local map before editing files in this folder or tracing a workflow through it.

## Stage In Lifecycle
Runtime and design. Structure notes connect file layout to behavior.

## Importance
Core structure note for understanding ownership and blast radius.

## Risks
The risk is reading this area in isolation and missing contracts owned by adjacent API, database, SDK, or runtime layers.

## Input / Output
| Input | Output |
| --- | --- |
| Folder/file evidence | Explanation of responsibility, dependencies, and workflow role |

## Resources
- [[Architecture]]
- [[Workflows/Product Learning Path]]
- [[Structure/Structure Index]]

## Related Topics
- [[Workflows/Real-Time Persona Session]]
- [[Workflows/Test-Driven Reading Path]]
- [[Concepts/Research Source Index]]

## Evidence
- modal_kflapi/app.py
- modal_kflapi/crypto.py
- modal_kflapi/openapi.json
- modal_kflapi_tests
