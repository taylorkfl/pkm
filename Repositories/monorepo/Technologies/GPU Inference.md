---
type: technology
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/technology
  - pkm/domain/ml
---

# GPU Inference

## Objective
Explain how GPU Inference is used in this repo, why it is present, when it runs, and what contracts it affects.

## Description
### What
Runtime execution of avatar/model workloads on capacity-managed GPU nodes.

### Why
The product depends on scarce GPU sessions, so admission control and reservations exist to protect inference capacity.

### When
Used whenever a session reserves a model namespace and dispatches to a node.

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
Runtime

## Importance
Core. This technology materially affects repo behavior in its domain.

## Risks
Risks include confusing generic technology behavior with this repo’s specific contract, relying on outdated provider docs, or changing one side of a cross-layer interface without updating the others.

## Input / Output
| Input | Output |
| --- | --- |
| Repo usage plus external docs | Technology-specific explanation tied to local evidence |

## Resources
- NVIDIA inference overview: https://developer.nvidia.com/deep-learning-performance-training-inference

## Related Topics
- [[Technologies/Technology Index]]
- [[Architecture]]
- [[Concepts/Research Source Index]]

## Evidence
- modal_kflapi/app.py
- supabase-kfl/supabase/migrations/20260109164203_nodes_and_reservations_rpcs.sql
