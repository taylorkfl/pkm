---
type: technology
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: medium
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/technology
  - pkm/domain/ai
---

# Cartesia

## Objective
Explain how Cartesia is used in this repo, why it is present, when it runs, and what contracts it affects.

## Description
### What
Alternative TTS provider supported by KFL voice-agent provider abstraction.

### Why
Provider abstraction keeps the turn loop stable while experimenting with voice latency/quality tradeoffs.

### When
Used when runtime config selects Cartesia TTS.

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
Medium. This technology materially affects repo behavior in its domain.

## Risks
Risks include confusing generic technology behavior with this repo’s specific contract, relying on outdated provider docs, or changing one side of a cross-layer interface without updating the others.

## Input / Output
| Input | Output |
| --- | --- |
| Repo usage plus external docs | Technology-specific explanation tied to local evidence |

## Resources
- Cartesia docs: https://docs.cartesia.ai/

## Related Topics
- [[Technologies/Technology Index]]
- [[Architecture]]
- [[Concepts/Research Source Index]]

## Evidence
- kfl-voice-agent/internal/voiceagent/tts_provider_factory.go
- kfl-voice-agent/ARCHITECTURE.md
