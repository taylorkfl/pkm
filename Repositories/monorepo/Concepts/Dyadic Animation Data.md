---
type: concept
repo: monorepo
status: growing
lifecycle_stage: research
importance: medium
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/research
---

# Dyadic Animation Data

## Objective
Define dyadic interaction data as the two-party timing, turn, interruption, transcript, audio, and expression signals needed to study conversational avatar behavior.

## Description
### What
Dyadic animation data is data about an interaction between two participants, not just one speaker. In this repo, dyadic signals include user audio, assistant audio, transcript roles, turn boundaries, interruption events, playback phase, emotion events, and alignment timing.

### Why
Conversational avatars are judged by interaction quality: turn-taking, response timing, interruption handling, expression appropriateness, and whether the avatar reacts to the user. Those behaviors require paired timelines.

### When
Use this concept when planning datasets, logging, evaluation, or research notes around interaction quality rather than isolated generation quality.

### How
Start by aligning event timelines: user speech -> STT endpoint -> assistant turn start -> audio chunks -> playback done/interrupted -> next user speech. Then ask what is logged, what is missing, and what privacy constraints apply.

## Scope
| Signal | Repo Surface |
| --- | --- |
| User audio | `input_audio_buffer.append`, microphone PCM |
| User text | `user_transcript` events |
| Assistant audio | `output_audio.chunk`, LiveKit byte stream |
| Assistant text | `assistant_done`, provider transcript events |
| Turn timing | `turn_start`, `turnEnd`, `playback_done` |
| Interruption | `interrupted`, playback phases, control data |
| Expression | `emotion` events and `setEmotion` control messages |

## Use Case
Use when connecting product runtime events to evaluation or model-training questions.

## Stage In Lifecycle
Research. It is a data-design concept that should be tied to runtime evidence.

## Importance
Medium. It supports future research quality but is not yet the main production contract.

## Risks
- Interaction data can contain sensitive audio/transcripts.
- Without clock synchronization, event timelines can misrepresent causality.
- Provider-specific alignment metadata may be incomplete.
- Logging more signal can affect latency or privacy posture.

## Input / Output
| Input | Output |
| --- | --- |
| Runtime events from both participants | Interaction timeline for analysis/evaluation |
| Research question | Data capture requirements and privacy constraints |

## Resources
- [[Philosophies/Dyadic Interaction Data]]
- [[Workflows/KFL Voice Agent Session]]
- [[Concepts/Emotion Conditioning]]
- [[Concepts/Representation Bottleneck]]
- [[Technologies/ElevenLabs]]
- Wav2Lip paper: https://arxiv.org/abs/2008.10010
- EMO paper: https://arxiv.org/abs/2402.17485

## Related Topics
- [[Workflows/S1 S2 Research Plan]]
- [[Concepts/Research Source Index]]
- [[Technologies/PyTorch]]

## Evidence
- js-elements/src/agents/types.ts
- js-elements/src/agents/elevenlabs.ts
- js-elements/src/PersonaEmbed.ts
- kfl-voice-agent/internal/voiceagent/session_core.go
- kfl-voice-agent/internal/voiceagent/stt.go
- kfl-voice-agent/internal/voiceagent/tts.go
