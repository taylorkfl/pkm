---
type: concept
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/realtime
---

# Voice AI Pipeline

## Objective
Explain what it means for a voice agent to call STT, LLM, and TTS services in this repo.

## Description
A voice AI pipeline turns user speech into assistant speech. STT converts microphone audio to text, the LLM decides what to say, and TTS converts the response text back into audio. The KFL voice agent implements this as a realtime streaming loop with interruption handling.

## Scope
| Stage | Meaning | Repo Evidence |
| --- | --- | --- |
| STT | Speech-to-text, audio in and transcript out | `kfl-voice-agent/internal/voiceagent/stt.go` |
| LLM | Large language model, transcript/context in and response text out | `kfl-voice-agent/internal/voiceagent/llm.go` |
| TTS | Text-to-speech, response text in and audio out | `kfl-voice-agent/internal/voiceagent/tts.go` |
| Browser WebSocket | Sends mic PCM to server and receives output audio chunks | `kfl-voice-agent/ARCHITECTURE.md` |
| Avatar media path | Agent audio drives the avatar session through LiveKit/Keyframe runtime | `js-elements/src/agents/kfl.ts`, `ml/windy-inference/session.py` |

## Use Case
Use this concept when reading `kfl-voice-agent`, debugging conversational latency, or distinguishing LiveKit media transport from the AI turn loop.

## Stage In Lifecycle
Runtime.

## Importance
Core. Voice behavior is one of the main realtime product paths.

## Risks
- STT, LLM, and TTS provider failures have different failure modes and latency budgets.
- Streaming cancellation/interruptions must prevent stale audio from continuing to play.
- Sample rates and audio formats must match between browser, agent, TTS, and avatar runtime.
- The media connection can be healthy while the AI provider loop is failing.

## Input / Output
| Input | Output |
| --- | --- |
| Browser microphone PCM | Transcript text, response text, and spoken audio chunks |
| Provider credentials and runtime config | Connected STT/LLM/TTS provider sessions |

## Resources
- [[Structure/kfl-voice-agent/kfl-voice-agent]]
- [[Definitions/LiveKit Agent]]
- [[Concepts/LiveKit Transport]]
- [[Technologies/Soniox]]
- [[Technologies/OpenRouter]]
- [[Technologies/ElevenLabs]]
- [[Technologies/Cartesia]]

## Related Topics
- [[Workflows/KFL Voice Agent Session]]
- [[Philosophies/Real-Time Streaming Pipeline]]
- [[Concepts/Voice Agent Details]]

## Evidence
- kfl-voice-agent/ARCHITECTURE.md
- kfl-voice-agent/internal/voiceagent/stt.go
- kfl-voice-agent/internal/voiceagent/llm.go
- kfl-voice-agent/internal/voiceagent/tts.go
- kfl-voice-agent/internal/voiceagent/session_core.go
- js-elements/src/agents/kfl.ts
