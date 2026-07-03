---
type: study-guide
status: current
last_reviewed: 2026-06-29
tags:
  - study
  - keyframelabs
  - avatars
  - demo-apps
  - docs
---

# Keyframe Labs Docs Study Guide - Demo Applications

## Objective

Build demo applications that show Keyframe Labs avatars in practical use cases: hosted browser demos first, React demos next, then self-managed, provider-specific, meeting, and LiveKit paths.

Docs companion: [[Keyframe Labs Docs Reading Map]]

Related existing notes:

- [[Study Plan - Keyframe Avatar Demo Apps]]
- [[Reading List - Keyframe Avatar Demo Apps]]

Docs checked: [Keyframe Labs docs](https://docs.keyframelabs.com/) and [docs index](https://docs.keyframelabs.com/llms.txt) on 2026-06-29.

## Fastest Hosted Demo Path

Goal: get a browser demo working before studying advanced integration paths.

- [ ] Start with the public docs index and quickstart path: [Keyframe docs](https://docs.keyframelabs.com/), [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Create or confirm platform access, then create the hosted deployment from the dashboard flow: [Get an API key](https://docs.keyframelabs.com/guides/getting-started/get-your-api-key), [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Choose hosted first when the demo can share provider credentials with Keyframe and use `PersonaEmbed`: [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Add every local and deployed demo URL to Allowed Origins before testing the publishable key: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Build the smallest possible HTML demo with start, stop, state logging, disconnect, and error logging: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).
- [ ] Smoke test microphone permission, connect, first response, stop, refresh, and tab close behavior: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).

Deliverable:

- [ ] One working hosted browser demo that can be explained end to end from publishable key to rendered persona: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).

## React Demo Apps

Goal: turn the minimal hosted demo into a reusable demo app shell.

- [ ] Read the JavaScript SDK page to understand where React sits in the package stack: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).
- [ ] Build the React demo on top of the React package when you want UI components and React bindings over Elements: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).
- [ ] Keep hosted React demos centered on `PersonaEmbed` behavior, not backend session creation: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Add state labels for idle, connecting, connected, listening, speaking, disconnected, and error by wiring SDK callbacks into React state: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).
- [ ] Add scenario controls for support, tutoring, roleplay, training, or meeting assistant demos after the single hosted flow works: [Introduction](https://docs.keyframelabs.com/), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).

Deliverable:

- [ ] Reusable React app shell with start/stop controls, status display, scenario selection, and a clear reset path: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript), [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).

## JavaScript SDK / Elements / React Wrapper

Goal: know which client layer to use for each demo.

- [ ] Use the low-level SDK only when you need to implement your own UI, state management, and provider binding: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).
- [ ] Use Elements when you want framework-agnostic primitives and a small amount of custom UI: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript), [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Use `PersonaEmbed` for hosted demos backed by a publishable key: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Use `PersonaView` for self-managed demos that pass Keyframe session details and provider details from your own infrastructure: [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed), [Self managed](https://docs.keyframelabs.com/guides/integrate/self-managed).
- [ ] Use the React package when the demo is a React app and you want React bindings over Elements: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).

Deliverable:

- [ ] A one-paragraph decision note for each demo explaining SDK vs Elements vs React: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript).

## Publishable Keys Vs Secret API Keys

Goal: keep demo credentials safe while still moving quickly.

- [ ] Use publishable keys only for hosted client embeds created from platform deployments and restricted by Allowed Origins: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Treat the Keyframe API key as server-side only; use it to call APIs such as session creation, voices, models, and meeting bots: [Get an API key](https://docs.keyframelabs.com/guides/getting-started/get-your-api-key), [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).
- [ ] Do not paste provider secrets into frontend code; hosted deployments store provider credentials in the Keyframe platform, while self-managed demos should generate provider credentials ephemerally: [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).
- [ ] For self-managed demos, create a tiny backend boundary before exposing voice, model, session, or meeting-bot configuration to the UI: [Self managed](https://docs.keyframelabs.com/guides/integrate/self-managed), [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).
- [ ] Before sharing a demo, verify production Allowed Origins and remove any test-only provider credentials from the browser bundle: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).

Deliverable:

- [ ] A credentials boundary note with two columns: browser-safe and server-only: [Get an API key](https://docs.keyframelabs.com/guides/getting-started/get-your-api-key), [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview).

## Personas, Voices, LLM Models, And Embed Configs

Goal: make demos configurable without making them fragile.

- [ ] Pick a current `persona-1.5-live` persona first; use deprecated `persona-1.0-live` personas only for legacy comparisons: [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library).
- [ ] Use `persona_slug` or `persona_id` consistently; session creation and meeting bots support either one, but each request should choose exactly one path: [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library), [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session), [Create meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot).
- [ ] For hosted demos, treat the hosted deployment plus publishable key plus Allowed Origins as the public embed configuration surface: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Read local platform notes only when you need internal embed-config behavior beyond the public docs: [[Repositories/monorepo/Concepts/Embed Config]], [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Use the voices API when a demo lets users choose voice or inspect voice metadata: [List voices](https://docs.keyframelabs.com/api-reference/voices/list-voices), [Get voice](https://docs.keyframelabs.com/api-reference/voices/get-voice).
- [ ] Use the LLM models API when a meeting bot or first-party voice-agent demo lets users choose model behavior: [List LLM models](https://docs.keyframelabs.com/api-reference/llm-models/list-llm-models), [Get LLM model](https://docs.keyframelabs.com/api-reference/llm-models/get-llm-model).
- [ ] For emotion-capable demos, prefer `persona-1.5-live` and test neutral, happy, sad, and angry expressions where supported: [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library), [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents), [ElevenLabs Agents hosted](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/elevenlabs-agents).

Deliverable:

- [ ] A demo configuration table listing persona, voice, LLM model, provider path, greeting, and safety notes for each demo: [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library), [List voices](https://docs.keyframelabs.com/api-reference/voices/list-voices), [List LLM models](https://docs.keyframelabs.com/api-reference/llm-models/list-llm-models).

## Session Creation

Goal: understand the server-created credentials required for self-managed demos.

- [ ] Read the session creation API before building `PersonaView` demos: [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).
- [ ] Learn the response fields the browser-side view needs: `server_url`, `participant_token`, and `agent_identity`: [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).
- [ ] Create sessions by persona ID or persona slug, never both in the same request: [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session), [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library).
- [ ] Keep session creation behind a backend endpoint so the secret API key never enters the browser: [Get an API key](https://docs.keyframelabs.com/guides/getting-started/get-your-api-key), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).
- [ ] Build retry behavior for documented session errors: plan not allowed, no capacity, internal error, and provider setup mistakes: [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).

Deliverable:

- [ ] A self-managed session flow sketch showing backend, Keyframe API, provider credentials, `PersonaView`, and browser media permissions: [Self managed](https://docs.keyframelabs.com/guides/integrate/self-managed), [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).

## Hosted Vs Self-Managed Integrations

Goal: choose the right architecture for each demo instead of defaulting to the most complex path.

- [ ] Use hosted when the goal is a fast, polished demo and sharing provider credentials with Keyframe is acceptable: [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Use self-managed when the demo needs maximum control, per-session avatar selection, or provider keys held on your own infrastructure: [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview), [Self managed](https://docs.keyframelabs.com/guides/integrate/self-managed).
- [ ] Use widget only when no-code configuration is enough; use hosted or self-managed for custom demo apps: [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview).
- [ ] Decide whether each demo needs per-session persona selection before choosing hosted or self-managed: [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview), [Self managed](https://docs.keyframelabs.com/guides/integrate/self-managed).

Deliverable:

- [ ] One architecture decision per demo: widget, hosted, or self-managed, with why: [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview).

## OpenAI Realtime And ElevenLabs Paths

Goal: know the fastest and most controllable provider routes.

- [ ] For hosted OpenAI Realtime, create a hosted deployment with OpenAI provider credentials and use the publishable key in `PersonaEmbed`: [OpenAI Realtime hosted](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/openai-realtime), [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] For self-managed OpenAI Realtime, generate an ephemeral OpenAI Realtime token server-side and pass provider details into `PersonaView`: [OpenAI Realtime self-managed](https://docs.keyframelabs.com/guides/self-managed/real-time-llms/openai-realtime), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).
- [ ] For hosted ElevenLabs, create a hosted deployment using the ElevenLabs agent ID and API key, then demo via `PersonaEmbed`: [ElevenLabs Agents hosted](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/elevenlabs-agents), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] For self-managed ElevenLabs, use the ElevenLabs agent ID plus signed URL as provider details for `PersonaView`: [ElevenLabs Agents self-managed](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/elevenlabs-agents), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).
- [ ] Add the ElevenLabs `set_emotion` client tool only after the basic agent path works: [ElevenLabs Agents hosted](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/elevenlabs-agents), [ElevenLabs Agents self-managed](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/elevenlabs-agents).

Deliverable:

- [ ] Provider comparison note: hosted OpenAI, self-managed OpenAI, hosted ElevenLabs, self-managed ElevenLabs: [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted), [Self managed](https://docs.keyframelabs.com/guides/integrate/self-managed).

## Meeting Bots / Zoom / Google Meet / Teams Demos

Goal: build meeting demos as a separate path from browser embeds.

- [ ] Read the meeting guide before using normal session assumptions; meeting personas join Zoom, Google Meet, and Microsoft Teams through Recall.ai: [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams).
- [ ] Collect required server-side inputs: meeting URL, persona, Recall API key, system prompt, greeting, voice ID, and LLM model ID: [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams), [Create meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot).
- [ ] Use voice and model list endpoints before exposing voice/model selectors in a meeting demo UI: [List voices](https://docs.keyframelabs.com/api-reference/voices/list-voices), [List LLM models](https://docs.keyframelabs.com/api-reference/llm-models/list-llm-models), [Create meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot).
- [ ] Track bot status after dispatch and show a clear waiting/admit state in the demo runbook: [Get meet bot status](https://docs.keyframelabs.com/api-reference/meet-bots/get-meet-bot-status), [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams).
- [ ] Add explicit cleanup steps for removing or stopping the meeting bot, and verify current stop endpoint details before live demos: [Stop meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/stop-meet-bot), [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams).

Deliverable:

- [ ] Meeting demo runbook with setup, dispatch, admit, status check, stop, and fallback plan: [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams), [Create meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot).

## LiveKit Agents Demos

Goal: use LiveKit only when the demo is intentionally self-managed or already LiveKit-based.

- [ ] Read the LiveKit Agents guide after hosted and basic self-managed flows are clear: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents), [Self managed](https://docs.keyframelabs.com/guides/integrate/self-managed).
- [ ] Add the Keyframe plugin to a LiveKit Voice AI app and set the Keyframe API key outside source control: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents), [Get an API key](https://docs.keyframelabs.com/guides/getting-started/get-your-api-key).
- [ ] Start `AvatarSession` from the LiveKit `AgentSession`, using exactly one of `persona_slug` or `persona_id`: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents), [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library).
- [ ] Test programmatic emotion control only with `persona-1.5-live` personas: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents), [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library).

Deliverable:

- [ ] LiveKit demo or architecture note showing agent session, Keyframe avatar session, persona choice, emotion path, and frontend preview path: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents).

## Local Development Requirements

Goal: avoid losing demo time to local server, origin, microphone, or credential issues.

- [ ] Serve the hosted quickstart from a local web server rather than opening the file directly: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Add `localhost` and any tunnel or deployed origins to Allowed Origins before using a publishable key: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Confirm microphone permission prompts are accepted and visible in browser settings: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).
- [ ] For self-managed demos, create provider and Keyframe credentials ephemerally before loading `PersonaView`: [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed), [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).
- [ ] For meeting demos, test with a disposable meeting room and known admission flow before a live audience: [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams), [Create meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot).

Deliverable:

- [ ] Local demo preflight checklist covering origin, mic, keys, provider config, browser console, and cleanup: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).

## Deployment And Demo Sharing

Goal: make demos shareable without leaking credentials or depending on laptop-only state.

- [ ] For hosted demos, add the production demo URL to Allowed Origins before sharing: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] For self-managed demos, deploy the backend session/token endpoint and store all secret API keys in the platform secret manager: [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed), [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).
- [ ] Share hosted demos first unless the demo value depends on per-session persona/provider customization: [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview), [Hosted](https://docs.keyframelabs.com/guides/integrate/hosted), [Self managed](https://docs.keyframelabs.com/guides/integrate/self-managed).
- [ ] Prepare a reset path for every public demo: disconnect browser sessions, stop meeting bots, and rotate any accidentally exposed test credentials: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Stop meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/stop-meet-bot), [Get an API key](https://docs.keyframelabs.com/guides/getting-started/get-your-api-key).

Deliverable:

- [ ] A short demo sharing note for each app: URL, allowed origin, provider path, required account resources, cleanup steps, and known failure modes: [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview), [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams).

## Testing And Troubleshooting

Goal: make demos repeatable enough to show more than once.

- [ ] Hosted smoke test: load, connect, grant mic, speak, hear response, stop, refresh, reconnect: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Self-managed smoke test: backend creates a session, browser receives ephemeral session details, provider token exists, `PersonaView` connects, disconnect cleans up: [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed), [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).
- [ ] Capture SDK state, agent state, disconnect, and error events while testing UI flows: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed).
- [ ] Map common failures to likely causes: bad origin, invalid publishable key, secret API key used in browser, missing mic permission, plan not allowed, no capacity, provider credential issue: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted), [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).
- [ ] Meeting smoke test: dispatch, admit, confirm camera feed, check status, remove or stop bot, confirm it leaves: [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams), [Get meet bot status](https://docs.keyframelabs.com/api-reference/meet-bots/get-meet-bot-status), [Stop meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/stop-meet-bot).
- [ ] LiveKit smoke test: plugin installed, API key available, avatar joins room, persona ID/slug is valid, emotion tool works only where supported: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents), [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library).

Deliverable:

- [ ] A reusable QA checklist for hosted, self-managed, meeting, and LiveKit demos: [Keyframe docs](https://docs.keyframelabs.com/), [Keyframe docs index](https://docs.keyframelabs.com/llms.txt).

## Suggested Build Order

- [ ] Hosted HTML demo: [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Hosted React demo shell: [JavaScript SDK reference](https://docs.keyframelabs.com/sdk-reference/javascript), [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Multi-scenario hosted demo with persona/voice/model decisions documented: [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library), [List voices](https://docs.keyframelabs.com/api-reference/voices/list-voices), [List LLM models](https://docs.keyframelabs.com/api-reference/llm-models/list-llm-models).
- [ ] Self-managed `PersonaView` demo with backend session creation: [Quickstart - self managed](https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed), [Create session](https://docs.keyframelabs.com/api-reference/sessions/create-session).
- [ ] Provider-specific demo, either OpenAI Realtime or ElevenLabs: [OpenAI Realtime hosted](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/openai-realtime), [ElevenLabs Agents hosted](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/elevenlabs-agents).
- [ ] Meeting bot proof-of-concept: [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams), [Create meet bot](https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot).
- [ ] LiveKit Agents demo: [LiveKit Agents](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents).

