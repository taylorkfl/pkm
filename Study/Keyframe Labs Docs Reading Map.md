---
type: reading-map
status: current
last_reviewed: 2026-06-29
tags:
  - study
  - keyframelabs
  - avatars
  - demo-apps
  - docs
---

# Keyframe Labs Docs Reading Map

## Purpose

Use this as the direct-link map for building Keyframe Labs avatar demo applications. The Keyframe Labs docs are first because they are the source of truth for public integration paths.

Companion guide: [[Keyframe Labs Docs Study Guide - Demo Applications]]

Related existing notes:

- [[Study Plan - Keyframe Avatar Demo Apps]]
- [[Reading List - Keyframe Avatar Demo Apps]]

Docs checked: [Keyframe Labs docs](https://docs.keyframelabs.com/) and [docs index](https://docs.keyframelabs.com/llms.txt) on 2026-06-29.

## Keyframe Labs Docs - Start Here

- [ ] Docs home and source index: [Keyframe Labs docs](https://docs.keyframelabs.com/), [llms.txt](https://docs.keyframelabs.com/llms.txt).
- [ ] Product framing and use cases: [Introduction](https://docs.keyframelabs.com/).
- [ ] API key creation and safety baseline: [Get an API key](https://docs.keyframelabs.com/guides/getting-started/get-your-api-key).
- [ ] Fastest hosted demo: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] First self-managed demo: [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).
- [ ] Client package choices: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).

## Integration Choice

- [ ] Compare widget, hosted, and self-managed before choosing demo architecture: [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview).
- [ ] Learn the hosted `PersonaEmbed` path for low-code demos: [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Learn the self-managed `PersonaView` path for custom demos: [Self managed](https://docs.keyframelabs.com/guides/integrate/self-managed).

## Hosted Demo Path

- [ ] Create a hosted deployment, configure provider credentials, set Allowed Origins, and copy the publishable key: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Understand why hosted demos share provider credentials with Keyframe and use a publishable key in the browser: [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Use `PersonaEmbed` for the first HTML demo and for hosted React demos: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).
- [ ] Test hosted provider variants after the generic quickstart works: [OpenAI Realtime hosted](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/openai-realtime), [ElevenLabs Agents hosted](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/elevenlabs-agents).

## React Demo Apps

- [ ] Read the JavaScript SDK page for package roles before starting React: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).
- [ ] Use React when the demo needs reusable components, UI state, form controls, and scenario modes: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).
- [ ] Keep the first React demo hosted until the UI is stable: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Move to self-managed React only when the app needs backend-created session credentials or per-session persona selection: [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed), [Self managed](https://docs.keyframelabs.com/guides/integrate/self-managed).

## JavaScript SDK / Elements / React Wrapper

- [ ] Low-level SDK: use when implementing your own UI and agent binding: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).
- [ ] Elements: use when you want framework-agnostic primitives such as `PersonaEmbed` and `PersonaView`: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).
- [ ] React: use when you want React bindings over Elements and reusable UI components: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).
- [ ] Hosted Elements path: `PersonaEmbed` plus publishable key: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Self-managed Elements path: `PersonaView` plus Keyframe session details and provider details: [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).

## Publishable Keys Vs Secret API Keys

- [ ] Create and store API keys from the platform dashboard: [Get an API key](https://docs.keyframelabs.com/guides/getting-started/get-your-api-key).
- [ ] Use publishable keys in hosted browser embeds only after Allowed Origins are configured: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Use secret API keys only from server-side code for sessions, voices, models, and meeting bots: [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session), [List voices](https://docs.keyframelabs.com/api-reference/voices/list-voices), [List LLM models](https://docs.keyframelabs.com/api-reference/llm-models/list-llm-models), [Create meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot).
- [ ] Re-check the hosted vs self-managed credential tradeoff before sharing a demo: [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted), [Self managed](https://docs.keyframelabs.com/guides/integrate/self-managed).

## Personas, Voices, LLM Models, And Embed Configs

- [ ] Browse current stock personas and prefer `persona-1.5-live` for new demos: [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library).
- [ ] Use persona ID or persona slug in session creation: [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session), [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library).
- [ ] Use persona ID or persona slug in meeting-bot creation: [Create meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot), [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library).
- [ ] Read voice catalog fields before adding a voice picker: [List voices](https://docs.keyframelabs.com/api-reference/voices/list-voices), [Get voice](https://docs.keyframelabs.com/api-reference/voices/get-voice).
- [ ] Read LLM model catalog fields before adding a model picker: [List LLM models](https://docs.keyframelabs.com/api-reference/llm-models/list-llm-models), [Get LLM model](https://docs.keyframelabs.com/api-reference/llm-models/get-llm-model).
- [ ] Treat hosted deployments, publishable keys, and Allowed Origins as the public docs version of embed configuration: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Read internal embed-config notes only when public docs are not enough: [[Repositories/monorepo/Concepts/Embed Config]], [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).

## Session Creation

- [ ] Read the create-session OpenAPI page before any self-managed build: [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).
- [ ] Confirm the response fields needed by the browser view: [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).
- [ ] Use self-managed integration docs to place session creation behind a backend boundary: [Self managed](https://docs.keyframelabs.com/guides/integrate/self-managed), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).
- [ ] Plan errors and retries from the documented API responses before live demos: [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).

## OpenAI Realtime And ElevenLabs Paths

- [ ] Hosted OpenAI Realtime: [OpenAI Realtime hosted](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/openai-realtime).
- [ ] Self-managed OpenAI Realtime: [OpenAI Realtime self-managed](https://docs.keyframelabs.com/guides/self-managed/real-time-llms/openai-realtime).
- [ ] Hosted ElevenLabs Agents: [ElevenLabs Agents hosted](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/elevenlabs-agents).
- [ ] Self-managed ElevenLabs Agents: [ElevenLabs Agents self-managed](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/elevenlabs-agents).
- [ ] Emotion control setup for ElevenLabs client tool: [ElevenLabs Agents hosted](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/elevenlabs-agents), [ElevenLabs Agents self-managed](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/elevenlabs-agents).

## Meeting Bots / Zoom / Google Meet / Teams

- [ ] Meeting overview and Recall.ai dependency: [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams).
- [ ] Create a meeting bot and required fields: [Create meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot).
- [ ] Observe bot status after dispatch: [Get meet bot status](https://docs.keyframelabs.com/api-reference/meet-bots/get-meet-bot-status).
- [ ] Stop or clean up a bot and verify behavior before live demos: [Stop meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/stop-meet-bot).
- [ ] Use voices and LLM model catalog pages to fill meeting-bot request fields: [List voices](https://docs.keyframelabs.com/api-reference/voices/list-voices), [List LLM models](https://docs.keyframelabs.com/api-reference/llm-models/list-llm-models).

## LiveKit Agents

- [ ] Read Keyframe's LiveKit Agents guide after self-managed basics: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents).
- [ ] Install the plugin and configure the Keyframe API key outside source control: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents), [Get an API key](https://docs.keyframelabs.com/guides/getting-started/get-your-api-key).
- [ ] Add `AvatarSession` to a LiveKit `AgentSession`: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents).
- [ ] Select exactly one persona identifier, slug or ID: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents), [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library).
- [ ] Test `set_emotion()` only with compatible `persona-1.5-live` personas: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents), [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library).

## Local Development Requirements

- [ ] Serve demos through a local web server and test in the browser: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).
- [ ] Add localhost, tunnel URLs, and production URLs to Allowed Origins as needed: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Check microphone permission and tab close behavior early: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).
- [ ] Generate session/provider credentials outside the browser for self-managed local demos: [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed), [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).

## Deployment And Demo Sharing

- [ ] Share hosted demos through a deployed URL whose origin is allowed in the hosted deployment: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Share self-managed demos only after backend session/token creation is deployed securely: [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed), [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).
- [ ] Include cleanup instructions for meetings and meeting bots: [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams), [Stop meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/stop-meet-bot).
- [ ] Re-read integration tradeoffs before deciding which demo should be public: [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview).

## Testing And Troubleshooting

- [ ] Hosted test checklist: connect, mic, first response, stop, refresh, reconnect: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Self-managed test checklist: session creation, provider credentials, `PersonaView` connection, disconnect cleanup: [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed), [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).
- [ ] API error checklist: plan not allowed, capacity, internal error, not found: [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session), [Get voice](https://docs.keyframelabs.com/api-reference/voices/get-voice), [Get LLM model](https://docs.keyframelabs.com/api-reference/llm-models/get-llm-model).
- [ ] Meeting test checklist: dispatch, admit, status, camera feed, stop: [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams), [Create meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot), [Get meet bot status](https://docs.keyframelabs.com/api-reference/meet-bots/get-meet-bot-status), [Stop meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/stop-meet-bot).
- [ ] LiveKit test checklist: plugin install, API key, avatar joins, valid persona, emotion behavior: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents), [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library).

## Local Obsidian Cross-Links

Use these after the public docs path is clear.

- [[Study Plan - Keyframe Avatar Demo Apps]]
- [[Reading List - Keyframe Avatar Demo Apps]]
- [[Repositories/monorepo/Concepts/Embed Config]]
- [[Repositories/monorepo/Workflows/API Key And Auth Flow]]
- [[Repositories/monorepo/Concepts/Voice AI Pipeline]]
- [[Repositories/monorepo/Concepts/LiveKit Transport]]

