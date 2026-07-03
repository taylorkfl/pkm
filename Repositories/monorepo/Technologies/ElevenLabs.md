---
type: technology
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/technology
  - pkm/domain/ai
---

# ElevenLabs

## Objective
Explain how ElevenLabs helps build demo applications in this repo, which ElevenLabs surface to choose, and what local runtime contracts it affects.

## Description
### Product chooser
| Surface | What it is | Best fit for demos like `demo` |
| --- | --- | --- |
| `ElevenCreative` | No-code browser product for creators to generate voiceovers, music, dubs, and studio-style media assets. | Useful only for manual preproduction: sample voices, intro clips, promo audio, or one-off assets. Not the main path for interactive demo apps. |
| `ElevenAgents` | Conversational agent platform with prompts, workflows, voices, turn-taking, tools, dynamic variables, web/mobile/telephony deployment, analytics, tests, and webhooks. | Primary choice for live persona demos. It owns the real-time voice conversation, conversation id, post-call transcript, and agent configuration. |
| `ElevenAPI` | REST APIs and Python/TypeScript SDKs for TTS, STT, speech engine, voice cloning, streaming, voices, and agent-related operations. | Use as the programmable layer around demos: signed URLs, custom TTS snippets, voice generation, streaming, transcription, and backend automation. |

Likely answer for new demo apps: start with `ElevenAgents` when the product experience is a live talking avatar or voice agent. Add `ElevenAPI` when the app needs backend glue, direct TTS, signed-url minting, batch creation, or custom audio. Use `ElevenCreative` only when a human is producing static media outside the application runtime.

### What
ElevenLabs is the third-party conversational voice and TTS provider behind many demo personas. In the current demos it is paired with Keyframe Personas: ElevenLabs supplies the voice-agent conversation, transcripts, audio chunks, alignment metadata, and post-call webhooks, while Keyframe renders the avatar and the demo backend prepares context, scores calls, and renders share results.

### Why
It gives the demo stack a high-quality, low-latency voice agent without building STT, LLM orchestration, TTS, turn-taking, and transcript capture from scratch. The repo then wraps that agent in product-specific logic such as upload-based briefings, generated first messages, phase nudges, multi-agent bridges, backchannel audio, OpenAI scoring, and public share cards.

### When
Use ElevenLabs whenever a demo needs one of these:

| Need | Local pattern |
| --- | --- |
| A live avatar conversation | `PersonaEmbed` gets an ElevenLabs signed URL and connects `ElevenLabsAgent`. |
| Per-session context | Backend creates a brief, then frontend injects dynamic variables like `{{founder_pre_read}}`, `{{first_message}}`, or `{{brief}}`. |
| Post-call scoring | Frontend registers `conversation_id`; ElevenLabs sends `post_call_transcription`; backend verifies HMAC, scores, stores, and frontend polls. |
| Agent-to-agent demo | `sendText` feeds one agent's final transcript to the other after `turnEnd`. |
| Long-running call steering | `sendContext` pushes non-interrupting phase/context updates during the call. |
| Mid-turn backchannels | Audio alignment maps character offsets to playback time; backend synthesizes tiny TTS clips directly through ElevenAPI. |

### How
The repo uses ElevenLabs at three layers:

| Layer | Responsibility | Key local files |
| --- | --- | --- |
| Agent auth/config | Store `agent_id`, mint signed URLs server-side, never expose API keys in browser code. | `modal_common/agents.py` |
| Browser adapter | Open the ElevenLabs WebSocket, send initiation data, send mic PCM, receive transcripts/audio/alignment, emit normalized Persona events. | `js-elements/src/agents/elevenlabs.ts`, `js-react/src/PersonaEmbed.tsx` |
| Demo product loop | Prepare context/openers, register conversation ids, process webhooks, poll results, share cards, synthesize backchannels. | `demo/src/App.tsx`, `demo/src/lib/kfldemoApi.ts`, `modal_kfldemoapi/app.py` |

### Demo builder checklist
1. Create or choose an ElevenLabs agent in ElevenAgents. Configure system prompt, first message, voice, LLM, language, turn-taking, interruptions, and any client tools.
2. Put per-demo variables into the agent prompt with `{{variable_name}}`. In this repo, demo-specific variables include `yc_application_context`, `founder_pre_read`, `brief`, and `first_message`.
3. Keep the ElevenLabs API key server-side. Browser sessions should use a signed URL or another browser-safe credential, not a raw API key.
4. Use a `conversation_id` as the join key between live UI state, backend rows, post-call webhooks, polling, and share pages.
5. For post-call results, enable `post_call_transcription`, verify `ElevenLabs-Signature`, tolerate additive fields, and return 200 after recording a handled failure so provider retries do not loop forever.
6. For reactive UI, prefer client tools for explicit client-side actions such as emotion, navigation, notifications, or DOM events. Use webhook/server tools when the agent needs to call backend APIs.
7. Use `contextual_update` for background facts that should not immediately trigger an agent turn; use user-message style input when the text should cause a reply.
8. For low-latency custom audio, use streaming or direct TTS through ElevenAPI. Eleven v3 is better for expressive character delivery; Flash v2.5 is the low-latency/cost-oriented real-time choice.
9. For multi-agent demos, wait for a reliable spoken-turn end signal before bridging transcripts. The local adapter builds a virtual audio-buffer drain signal because `agent_response` arrives before playback finishes.
10. Treat direct audio snippets such as Lightcone backchannels as side-channel audio, not conversation turns, when they should not enter the agent's memory.

## Scope
| Lens | What To Learn |
| --- | --- |
| Product selection | When to use ElevenCreative, ElevenAgents, or ElevenAPI |
| Runtime integration | How browser, backend, and ElevenLabs coordinate live conversations |
| Demo features | Dynamic variables, webhooks, signed URLs, TTS snippets, client tools, alignment, and polling |
| Failure modes | What breaks when auth, timing, webhook, or agent prompt boundaries are misunderstood |

## Use Case
Use this note before creating a new voice demo, changing a persona's ElevenLabs agent, wiring post-call scorecards, adding a custom audio feature, or deciding whether a feature belongs in ElevenAgents dashboard config or in backend/frontend code.

## Stage In Lifecycle
Runtime

## Importance
Core. ElevenLabs is the main third-party voice-agent surface used by the current demo app pattern.

## Risks
- `ElevenCreative` is not a runtime integration surface. Choosing it for a live demo will leave you without signed URLs, WebSocket events, dynamic variables, or webhooks.
- Exposing `ELEVENLABS_API_KEY` to browser code would leak server authority. Mint signed URLs on the server.
- Changing ElevenLabs agent dashboard placeholders without updating frontend dynamic-variable names can silently produce generic conversations.
- `agent_response` text arrives before all audio is done playing. Bridging two agents at that point can make them talk over each other.
- Webhook handlers must verify HMAC signatures and handle additive schema changes. The provider docs explicitly warn that extra fields can be added.
- Returning non-200 responses repeatedly can disable webhooks; recording handled failures and returning 200 is safer for demo polling UX.
- Direct TTS snippets can pollute conversation memory if routed through the agent instead of side-channel Persona audio.
- Model naming changes matter. The docs recommend Flash models over older Turbo models for low-latency use, while local code/comments may still mention Turbo-era assumptions.

## Input / Output
| Input | Output |
| --- | --- |
| Agent id plus signed URL | Browser-safe live ElevenLabs conversation |
| Dynamic variables | Personalized prompt/first-message/tool context for one demo session |
| User mic PCM or text message | Agent transcript, audio chunks, alignment, and state events |
| Post-call transcription webhook | Stored score/result/share payload after backend processing |
| Direct TTS request | Audio bytes for side-channel playback or generated assets |

## Resources
- ElevenLabs documentation overview: https://elevenlabs.io/docs/overview/intro
- ElevenLabs models overview: https://elevenlabs.io/docs/overview/models
- ElevenAgents overview: https://elevenlabs.io/docs/eleven-agents/overview
- ElevenAgents React SDK: https://elevenlabs.io/docs/eleven-agents/libraries/react
- ElevenAgents WebSocket docs: https://elevenlabs.io/docs/eleven-agents/libraries/web-sockets
- ElevenAgents dynamic variables: https://elevenlabs.io/docs/eleven-agents/customization/personalization/dynamic-variables
- ElevenAgents client tools: https://elevenlabs.io/docs/eleven-agents/customization/tools/client-tools
- ElevenAgents post-call webhooks: https://elevenlabs.io/docs/eleven-agents/workflows/post-call-webhooks
- ElevenAPI quickstart: https://elevenlabs.io/docs/eleven-api/quickstart
- ElevenAPI audio streaming concepts: https://elevenlabs.io/docs/eleven-api/concepts/audio-streaming

## Related Topics
- [[Technologies/Technology Index]]
- [[Architecture]]
- [[Structure/demo/demo]]
- [[Structure/js-elements/js-elements]]
- [[Structure/modal_kfldemoapi/modal_kfldemoapi]]
- [[Concepts/Per-Session Context]]
- [[Concepts/Dynamic Variables]]
- [[Concepts/Webhooks]]
- [[Concepts/Voice Agent Details]]
- [[Workflows/Demo Scoring And Share Flow]]
- [[Concepts/Research Source Index]]

## Evidence
- modal_common/agents.py
- js-elements/src/agents/elevenlabs.ts
- js-elements/src/PersonaEmbed.ts
- js-react/src/PersonaEmbed.tsx
- demo/src/App.tsx
- demo/src/lib/kfldemoApi.ts
- modal_kfldemoapi/app.py
- kfl-voice-agent/internal/voiceagent/tts_provider_elevenlabs.go
