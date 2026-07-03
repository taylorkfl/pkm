---
type: technology
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/technology
  - pkm/domain/runtime
---

# WebSockets

## Objective
Explain how WebSockets is used in this repo, why it is present, when it runs, and what contracts it affects.

## Description
### What
Full-duplex event and audio transport used by voice providers and the KFL voice-agent runtime.

### Why
Voice agents need bidirectional streaming: microphone chunks up, events/audio chunks down.

### When
Used by KFL browser adapter, KFL Go server, Soniox STT, [[Technologies/ElevenLabs]], [[Technologies/OpenAI Realtime]], and TTS providers.

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
High. This technology materially affects repo behavior in its domain.

## Risks
Risks include confusing generic technology behavior with this repo’s specific contract, relying on outdated provider docs, or changing one side of a cross-layer interface without updating the others.

## Input / Output
| Input | Output |
| --- | --- |
| Repo usage plus external docs | Technology-specific explanation tied to local evidence |

## Resources
- WebSocket protocol RFC 6455: https://www.rfc-editor.org/rfc/rfc6455
- ElevenAgents WebSocket docs: https://elevenlabs.io/docs/eleven-agents/libraries/web-sockets

## Related Topics
- [[Technologies/Technology Index]]
- [[Technologies/ElevenLabs]]
- [[Architecture]]
- [[Concepts/Research Source Index]]

## Evidence
- kfl-voice-agent/internal/voiceagent/*.go
- js-elements/src/agents/*.ts
