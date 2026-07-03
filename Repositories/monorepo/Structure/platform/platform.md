---
type: structure-note
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/structure
---

# platform

## Objective
Explain the `platform` repo area as part of the larger realtime persona product architecture.

## Description
### What
React dashboard for org/product workflows including hosted agents, widget builder, API keys, usage, billing, personas, and deployment configuration.

### Why
The platform turns product concepts into API calls and persisted state. The widget builder especially mirrors server lifecycle rules for drafts, edit drafts, publish, deactivate, and discard.

### When
Read this note when a change touches this folder or when a workflow note points here as evidence.

### How
User/org context -> form state/store -> authed API calls -> Supabase/API responses -> UI state and published embed keys.

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
- platform/src/App.tsx
- platform/src/features/agents/useWidgetBuilderStore.ts
- platform/src/features/agents/*
- platform/src/pages/*
