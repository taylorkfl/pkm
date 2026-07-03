---
type: workflow
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/workflow
  - pkm/domain/runtime
---

# KFL Voice Agent Session

## Objective
Explain the first-party voice-agent runtime from browser WebSocket input through STT, LLM, TTS, interruption, emotion, and playback-state events.

## Description
### What
The KFL voice agent is a Go WebSocket runtime that turns browser microphone PCM into a spoken assistant response. It connects to Soniox for STT, OpenRouter for streaming LLM output, and a TTS provider such as ElevenLabs or Cartesia. It emits JSON events and base64 audio chunks back to the browser adapter.

### Why
First-party orchestration gives the repo control over endpointing, interruption, prompt/emotion policy, TTS provider choice, and conversational memory. It also creates a single event contract that the browser adapter can normalize into the same `Agent` interface used by third-party agents.

### When
It runs when the embed config uses `voice_agent.type = kfl`, or when `/v1/sessions/kfl` creates a direct KFL session. Draft previews can pass signed config-id claims; published sessions can resolve config by publishable key.

### How
The runtime starts one session per WebSocket. `run()` connects STT and TTS, emits `session.ready`, launches concurrent client and STT readers, plays a greeting, and enters a turn loop. STT final tokens accumulate until `<end>`, endpointed transcripts are coalesced, the LLM streams tokens into TTS, and TTS audio events become `turn_start`, `output_audio.chunk`, `output_audio.done`, and `playback_done`. Speech during priming/streaming/tail phases triggers interruption.

## Scope
| Runtime Concern | Implementation Detail | Why It Exists |
| --- | --- | --- |
| Endpoint coalescing | Bounded queue merges quick endpoint bursts | Prevents rapid follow-ups from being lost while a turn is busy |
| Playback phases | idle, priming, streaming, tail | Allows barge-in during generated audio and buffered tail playback |
| Emotion extraction | LLM must start with `[angry]`, `[sad]`, `[happy]`, or `[neutral]` | Keeps expression state out of spoken TTS text while driving avatar emotion |
| Heard-text memory | Estimate heard chars from alignment/timing after interruption | Conversation memory should reflect what the user actually heard |
| Provider abstraction | `TTSProvider` interface | Lets ElevenLabs and Cartesia fit the same turn loop |
| Signed config source | HMAC token with reservation/config claims | Keeps browser auth short-lived and binds runtime config to session context |

## Use Case
Use when debugging KFL voice sessions, barge-in, missing audio, prompt behavior, emotion events, STT endpoint delay, TTS provider errors, or draft preview sessions.

## Stage In Lifecycle
Runtime. This is the live conversation loop for first-party voice sessions.

## Importance
Core. It controls the user-perceived intelligence and responsiveness of KFL-hosted agents.

## Risks
- Provider APIs have different sample rates, event formats, latency behavior, and failure semantics.
- If interruption does not abort TTS and cancel the LLM request, stale audio can continue after the user starts speaking.
- If the assistant transcript stores full generated text after interruption, future turns may reference words the user never heard.
- Endpoint coalescing can hide bugs if transcript limits are too small or queue behavior drops the wrong utterance.
- Emotion tags are prompt-driven; malformed tags need fallback stripping so TTS does not speak markup.

## Input / Output
| Input | Output |
| --- | --- |
| Browser event `input_audio_buffer.append` with base64 PCM | Binary PCM frames to Soniox STT |
| STT final tokens and `<end>` marker | Coalesced user transcript |
| User transcript and conversation memory | Streaming LLM tokens |
| LLM tokens | TTS text chunks, then PCM audio chunks |
| Playback interruption | `interrupted` event, aborted TTS context, truncated assistant memory |

## Resources
- [[Technologies/Go]]
- [[Technologies/WebSockets]]
- [[Technologies/Soniox]]
- [[Technologies/OpenRouter]]
- [[Technologies/ElevenLabs]]
- [[Technologies/Cartesia]]
- [[Concepts/Emotion Conditioning]]
- [[Philosophies/Real-Time Streaming Pipeline]]
- WebSocket protocol RFC 6455: https://www.rfc-editor.org/rfc/rfc6455
- Soniox realtime STT docs: https://soniox.com/docs/stt/rt/overview
- OpenRouter streaming docs: https://openrouter.ai/docs/api-reference/streaming
- ElevenAgents WebSocket docs: https://elevenlabs.io/docs/eleven-agents/libraries/web-sockets
- Cartesia docs: https://docs.cartesia.ai/

## Related Topics
- [[Structure/kfl-voice-agent/kfl-voice-agent]]
- [[Workflows/Real-Time Persona Session]]
- [[Concepts/Voice Agent Details]]
- [[Philosophies/Emotion-Conditioned Generation]]
- [[Concepts/Research Source Index]]

## Evidence
- kfl-voice-agent/ARCHITECTURE.md
- kfl-voice-agent/internal/voiceagent/session_core.go
- kfl-voice-agent/internal/voiceagent/stt.go
- kfl-voice-agent/internal/voiceagent/llm.go
- kfl-voice-agent/internal/voiceagent/tts.go
- kfl-voice-agent/internal/voiceagent/embed_config.go
- kfl-voice-agent/internal/voiceagent/protocol.go
- kfl-voice-agent/internal/voiceagent/session_interrupt_test.go
- kfl-voice-agent/testdata/scenarios/CONTRACT.md
