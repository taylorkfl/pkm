---
type: workflow
repo: monorepo
status: growing
lifecycle_stage: research
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/workflow
  - pkm/domain/research
---

# S1 S2 Research Plan

## Objective
Frame the research-side reading path around representation split, emotion conditioning, speech/audio alignment, and dyadic interaction data.

## Description
### What
The research spine is about separating high-level interaction intent from low-level audiovisual realization. In the existing notes this is captured as S1/S2 representation split, emotion-conditioned generation, representation bottlenecks, and dyadic interaction data.

### Why
Realtime avatar products need more than text-to-speech. They need timing, expression, interruption, gaze/turn-taking assumptions, and mappings from speech/audio to visible motion. The repo’s runtime already exposes product-level hooks for emotion, alignment, interruption, and turn boundaries that research systems can eventually exploit.

### When
Use this path when evaluating model choices, adding richer emotion/expression controls, designing data capture, or deciding where a research concept should surface in product code.

### How
Read current product contracts first: `Emotion` values, audio alignment events, KFL emotion tags, STT endpointing, and Persona session control messages. Then read research notes and papers to understand what would need to become explicit data, model input, or evaluation signal.

## Scope
| Research Question | Repo Signal | External Anchor |
| --- | --- | --- |
| How should speech content become facial/motion representation? | `sendAudio`, `setEmotion`, avatar control topics | Wav2Lip, EMO |
| Where should emotion be represented? | LLM tag, adapter emotion event, Persona control message | Emotion-conditioned generation literature |
| What belongs in high-level vs low-level representation? | S1/S2 notes, alignment events, runtime events | representation learning and speech model papers |
| What is dyadic interaction data? | interruption, transcript, turn boundaries, playback states | conversational turn-taking and dialogue system evaluation |

## Use Case
Use when connecting implementation decisions to research assumptions or when creating a research-backed implementation plan.

## Stage In Lifecycle
Research. It connects runtime affordances to model/data questions.

## Importance
High. Research abstractions shape future product capabilities and must stay grounded in actual runtime contracts.

## Risks
- Research language can become too broad unless tied to concrete repo inputs and outputs.
- Product emotion labels are currently coarse and may not support richer expression without schema/protocol changes.
- Audio alignment quality affects any downstream claim about “what was heard” or fine-grained expression timing.
- Papers can support directions, but repo evidence must decide what exists today.

## Input / Output
| Input | Output |
| --- | --- |
| Runtime event, data type, or model question | Research note explaining what it is, why it matters, and how it maps to code |
| Paper/article finding | Cautious claim tied to repo evidence and future work |

## Resources
- [[Concepts/Representation Bottleneck]]
- [[Concepts/Emotion Conditioning]]
- [[Concepts/Dyadic Animation Data]]
- [[Technologies/ElevenLabs]]
- [[Philosophies/S1 S2 Representation Split]]
- [[Philosophies/Emotion-Conditioned Generation]]
- [[Philosophies/Dyadic Interaction Data]]
- WavLM paper: https://arxiv.org/abs/2110.13900
- Wav2Lip paper: https://arxiv.org/abs/2008.10010
- EMO paper: https://arxiv.org/abs/2402.17485
- PyTorch paper: https://papers.neurips.cc/paper/9015-pytorch-an-imperative-style-high-performance-deep-learning-library

## Related Topics
- [[Workflows/KFL Voice Agent Session]]
- [[Concepts/LiveKit Transport]]
- [[Technologies/PyTorch]]
- [[Technologies/GPU Inference]]
- [[Concepts/Research Source Index]]

## Evidence
- js-sdk/src/PersonaSession.ts
- js-elements/src/agents/types.ts
- js-elements/src/agents/elevenlabs.ts
- kfl-voice-agent/internal/voiceagent/llm.go
- kfl-voice-agent/internal/voiceagent/tts.go
- kfl-voice-agent/testdata/scenarios/CONTRACT.md
- pyproject.toml
