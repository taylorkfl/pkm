---
type: technology
repo: monorepo
status: seedling
lifecycle_stage: operations
importance: medium
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/technology
  - pkm/domain/infrastructure
---

# Cloudflare R2

## Objective
Explain how Cloudflare R2 is used in this repository.

## Description
Object storage path referenced by GPU/checkpoint and generated media workflows.

## Scope
Repo-specific usage, ownership, risks, and related workflows. Generic external documentation is intentionally out of scope unless needed later.

## Use Case
Use this note when code, manifests, tests, or workflows mention Cloudflare R2.

## Stage In Lifecycle
Operations

## Importance
Medium

## Risks
Needs investigation whenever this technology boundary changes because it can affect connected workflows and deployment assumptions.

## Input / Output
| Input | Output |
| --- | --- |
| Cloudflare R2 usage in source files | Repo-specific explanation and navigation links |

## Resources
- `ml/InfiniteTalk/README.md`
- `pyproject.toml`

## Related Topics
- [[Technologies/GPU Inference]]
- [[Technologies/Technology Index]]

## Evidence
- ml/InfiniteTalk/README.md
- pyproject.toml
