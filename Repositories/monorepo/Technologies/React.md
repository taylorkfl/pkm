---
type: technology
repo: monorepo
status: growing
lifecycle_stage: design
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/technology
  - pkm/domain/frontend
---

# React

## Objective
Explain how React is used in this repo, why it is present, when it runs, and what contracts it affects.

## Description
### What
Frontend UI library for platform and demo apps.

### Why
It supports componentized dashboard workflows and stateful widget-builder UX.

### When
Used in `platform` and `demo` apps.

### How
Read the repo files listed in Evidence first, then use the external source links to understand the underlying technology behavior. Treat external docs as support for protocol/framework behavior and repo evidence as the source of truth for local implementation.

## Scope
| Lens | What To Learn |
| --- | --- |
| Repo role | Which workflow depends on this technology |
| Contract surface | Types, events, endpoints, tables, tokens, or commands it shapes |
| Failure modes | What breaks when the technology boundary is misunderstood |
| External basis | Official docs, standards, or papers that explain the underlying mechanism |

## Use Case
Use when a workflow note links here or when you encounter this technology in code and need the repo-specific mental model.

## Stage In Lifecycle
Design

## Importance
High. This technology materially affects repo behavior in its domain.

## Risks
Risks include confusing generic technology behavior with this repo’s specific contract, relying on outdated provider docs, or changing one side of a cross-layer interface without updating the others.

## Input / Output
| Input | Output |
| --- | --- |
| Repo usage plus external docs | Technology-specific explanation tied to local evidence |

## Resources
- React Learn docs: https://react.dev/learn

## Related Topics
- [[Technologies/Technology Index]]
- [[Architecture]]
- [[Concepts/Research Source Index]]

## Evidence
- platform/src/App.tsx
- demo/src/App.tsx
