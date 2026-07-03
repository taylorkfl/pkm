---
type: concept
repo: monorepo
status: growing
lifecycle_stage: research
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/research
---

# Representation Bottleneck

## Objective
Explain the research idea that intermediate representations can either preserve or discard timing, expression, identity, and interaction signals needed by avatar generation.

## Description
### What
A representation bottleneck is the point where rich input signals become a narrower intermediate form. In this repo, likely bottlenecks include text transcripts, emotion labels, PCM chunks, alignment metadata, LiveKit control messages, and any future S1/S2 latent representation.

### Why
Every bottleneck chooses what the model/runtime can still know. Text loses prosody. Coarse emotion labels lose nuance. Audio alone may not capture intent or turn-taking context. Alignment metadata helps recover timing but may be provider-specific.

### When
This matters whenever research capabilities are converted into product protocols: adding expression control, improving lip sync, using dyadic context, or deciding how much signal to send from voice-agent runtime to avatar renderer.

### How
Identify each boundary, list what is preserved, what is discarded, and what downstream component assumes. Then connect those assumptions to tests or papers before changing protocol shape.

## Scope
| Bottleneck | Preserved | Lost/Risky |
| --- | --- | --- |
| Transcript text | Semantic content | Prosody, timing, hesitation, emotion intensity |
| Emotion enum | Coarse affect | Mixed emotions, valence/arousal, subtle expression |
| PCM audio | Prosody and timing | Text semantics unless separately transcribed |
| Alignment metadata | Character timing | Provider-specific quality and availability |
| LiveKit control messages | Low-latency commands | Reliability if sent as unreliable data |

## Use Case
Use when evaluating research-to-product interfaces or deciding whether a feature needs richer runtime data.

## Stage In Lifecycle
Research. It shapes future model and protocol design.

## Importance
High. Bad bottlenecks create model ceilings that are hard to fix later.

## Risks
- Research claims can overfit to papers if not grounded in actual repo signal paths.
- Increasing representation richness increases schema, protocol, and latency complexity.
- Provider-specific metadata can make abstractions leaky.
- Coarse representations are easier to ship but may block future expressivity.

## Input / Output
| Input | Output |
| --- | --- |
| Runtime signal or proposed representation | Analysis of what information crosses the boundary |
| Research paper concept | Repo-specific implication and implementation question |

## Resources
- [[Philosophies/S1 S2 Representation Split]]
- [[Concepts/Emotion Conditioning]]
- [[Concepts/Dyadic Animation Data]]
- [[Technologies/ElevenLabs]]
- WavLM paper: https://arxiv.org/abs/2110.13900
- Wav2Lip paper: https://arxiv.org/abs/2008.10010
- EMO paper: https://arxiv.org/abs/2402.17485
- PyTorch paper: https://papers.neurips.cc/paper/9015-pytorch-an-imperative-style-high-performance-deep-learning-library

## Related Topics
- [[Workflows/S1 S2 Research Plan]]
- [[Technologies/PyTorch]]
- [[Technologies/GPU Inference]]
- [[Concepts/Research Source Index]]

## Evidence
- js-elements/src/agents/types.ts
- js-elements/src/agents/elevenlabs.ts
- js-sdk/src/PersonaSession.ts
- kfl-voice-agent/internal/voiceagent/llm.go
- kfl-voice-agent/internal/voiceagent/tts.go
- kfl-voice-agent/testdata/scenarios/CONTRACT.md
