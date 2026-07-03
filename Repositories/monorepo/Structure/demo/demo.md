---
type: structure-note
repo: monorepo
status: seedling
lifecycle_stage: runtime
importance: medium
last_reviewed: 2026-06-28
repo_path: demo
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/product
---

# demo

## Objective
Explain the responsibility and learning entry points for `demo`.

## Description
React/Vite demo experience using PersonaEmbed plus demo-specific context prep, scoring, polling, and result UI.

## Scope
This note covers `demo` as a repo area. It does not replace workflow notes or line-by-line code reading.

## Use Case
Use before making changes in this directory or when following a workflow into this subsystem.

## Stage In Lifecycle
Runtime

## Importance
Medium

## Risks
Changing this area can affect linked workflows and should be checked against related tests or API contracts.

## Input / Output
| Input | Output |
| --- | --- |
| Files under `demo` | Exports, routes, configs, UI behavior, schema, or runtime behavior described by this area |

## Resources
- `demo/package.json`
- `demo/src/App.tsx`
- `demo/src/lib/kfldemoApi.ts`
- `demo/src/InterviewDemoApp.tsx`

## Related Topics
- [[Workflows/Demo Scoring And Share Flow]]
- [[Structure/Structure Index]]

## Evidence
- demo/package.json
- demo/src/App.tsx
- demo/src/lib/kfldemoApi.ts
- demo/src/InterviewDemoApp.tsx
