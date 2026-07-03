---
type: structure-note
repo: monorepo
repo_path: ml/InfiniteTalk
status: growing
lifecycle_stage: runtime
importance: medium
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/ml
---

# InfiniteTalk

## Objective
Explain the queued GPU video generation worker under `ml/InfiniteTalk`.

## Description
`ml/InfiniteTalk` is a GPU worker for processing video generation jobs from a Supabase job queue. The worker claims pending jobs, downloads input audio/assets, runs generation, uploads outputs, and updates job status.

## Scope
| Path | Role |
| --- | --- |
| `ml/InfiniteTalk/README.md` | Worker architecture and deployment overview |
| `ml/InfiniteTalk/worker.py` | Queue polling and processing worker |
| `ml/InfiniteTalk/justfile` | Remote deployment and operations commands |
| `ml/InfiniteTalk/deploy.sh` | Deployment helper |
| `ml/InfiniteTalk/wan` | Model/runtime code |

## Use Case
Use this note when working on queued video jobs rather than realtime avatar inference.

## Stage In Lifecycle
Runtime and ML operations.

## Importance
Medium. It is a separate ML worker path from real-time persona sessions.

## Risks
- Job claiming and retry logic must avoid duplicate processing.
- GPU host setup and remote deployment assumptions are outside normal web local dev.
- R2/Supabase credentials are required for real jobs.

## Input / Output
| Input | Output |
| --- | --- |
| Pending Supabase video job and input media | Generated video uploaded to storage and job status update |

## Resources
- [[Technologies/GPU Inference]]
- [[Technologies/PyTorch]]
- [[Technologies/Supabase]]
- [[Technologies/Cloudflare R2]]

## Related Topics
- [[Structure/ml/windy-inference/windy-inference]]
- [[Concepts/Database Migrations And Seeding]]

## Evidence
- ml/InfiniteTalk/README.md
- ml/InfiniteTalk/worker.py
- ml/InfiniteTalk/justfile
- ml/InfiniteTalk/deploy.sh
