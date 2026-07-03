---
type: structure-note
repo: monorepo
status: seedling
lifecycle_stage: runtime
importance: medium
last_reviewed: 2026-06-28
repo_path: livekit-plugins-keyframe
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/integrations
---

# livekit-plugins-keyframe

## Objective
Explain the responsibility and learning entry points for `livekit-plugins-keyframe`.

## Description
Python LiveKit Agents plugin that starts Keyframe avatar sessions from LiveKit agent sessions.

## Scope
This note covers `livekit-plugins-keyframe` as a repo area. It does not replace workflow notes or line-by-line code reading.

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
| Files under `livekit-plugins-keyframe` | Exports, routes, configs, UI behavior, schema, or runtime behavior described by this area |

## Resources
- `livekit-plugins-keyframe/README.md`
- `livekit-plugins-keyframe/pyproject.toml`
- `livekit-plugins-keyframe/livekit/plugins/keyframe`

## Related Topics
- [[Technologies/LiveKit]]
- [[Concepts/LiveKit Transport]]
- [[Structure/Structure Index]]

## Evidence
- livekit-plugins-keyframe/README.md
- livekit-plugins-keyframe/pyproject.toml
- livekit-plugins-keyframe/livekit/plugins/keyframe
