---
type: concept
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/research
---

# Emotion Conditioning

## Objective
Explain how coarse emotion labels move from voice-agent reasoning into avatar expression control, and where research assumptions enter product code.

## Description
### What
Emotion conditioning in this repo currently means a small set of labels: `neutral`, `angry`, `sad`, and `happy`. Those labels can be emitted by OpenAI tool calls, ElevenLabs client tools, or the KFL runtime’s LLM emotion tag, then sent to the Persona session as avatar control data.

### Why
A talking avatar needs more than audio. Emotion labels are a compact control signal for expression and demeanor. They are intentionally coarse because they need to pass through multiple providers and runtime layers reliably.

### When
Emotion is set during assistant turns, when the voice agent infers response tone or provider tooling calls `set_emotion`.

### How
The generic agent interface emits `emotion`. `PersonaEmbed` listens and calls `PersonaSession.setEmotion()`. The session publishes unreliable `lk.control` data with `emotion:<label>` to the agent/avatar identity. The KFL runtime additionally prompts the LLM to start with an emotion tag and strips it before TTS.

## Scope
| Layer | Emotion Mechanism |
| --- | --- |
| Agent type contract | `Emotion = neutral | angry | sad | happy` |
| OpenAI adapter | Function tool `set_emotion` emits label |
| ElevenLabs adapter | Client tool call `set_emotion` emits label |
| KFL runtime | LLM tag parsed, emitted, removed from spoken text |
| Persona session | `emotion:<label>` control data over LiveKit |

## Use Case
Use when changing avatar expressions, adding new emotion labels, or connecting research work to runtime control messages.

## Stage In Lifecycle
Runtime and research. It is a runtime control path with research implications.

## Importance
High. Emotion is user-visible and model-facing, so mismatches are obvious and expensive to debug.

## Risks
- Adding labels requires changes across TypeScript types, provider tools, KFL prompt/parser, avatar model/control protocol, and tests.
- Unreliable control packets can be dropped; expression state should tolerate missed updates.
- Prompt-based emotion tags can be malformed, missing, or misplaced.
- Coarse labels may not capture nuanced affect needed by future research/modeling.

## Input / Output
| Input | Output |
| --- | --- |
| Provider tool call or KFL LLM emotion tag | Normalized `emotion` event |
| Normalized event | LiveKit control packet directed to avatar agent |

## Resources
- [[Philosophies/Emotion-Conditioned Generation]]
- [[Workflows/KFL Voice Agent Session]]
- [[Concepts/LiveKit Transport]]
- EMO paper: https://arxiv.org/abs/2402.17485
- Wav2Lip paper: https://arxiv.org/abs/2008.10010

## Related Topics
- [[Concepts/Representation Bottleneck]]
- [[Technologies/OpenAI Realtime]]
- [[Technologies/ElevenLabs]]
- [[Concepts/Research Source Index]]

## Evidence
- js-elements/src/agents/types.ts
- js-elements/src/agents/openai-realtime.ts
- js-elements/src/agents/elevenlabs.ts
- js-elements/src/agents/kfl.ts
- js-sdk/src/PersonaSession.ts
- kfl-voice-agent/internal/voiceagent/llm.go
