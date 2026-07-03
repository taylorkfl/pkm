---
type: study-note
status: growing
last_reviewed: 2026-06-29
tags:
  - study
  - keyframelabs
  - avatars
  - demo-apps
---

# Reading List - Keyframe Avatar Demo Apps

## Objective

Prioritize the docs, codebase notes, books, and technical subjects that most directly help build demo applications showing [[Repositories/monorepo/Concepts/Persona|Keyframe personas]] in different use cases.

## How To Use This List

Work from top to bottom. The early items unblock a working avatar demo fastest; the later items deepen local development, backend, deployment, testing, and media-system understanding.

Related plan: [[Study Plan - Keyframe Avatar Demo Apps]]

Docs-focused map: [[Keyframe Labs Docs Reading Map]]

## Priority Reading Checklist

### 1. Keyframe Labs Product And Quickstarts

- [ ] [Keyframe Labs docs](https://docs.keyframelabs.com/) - start here first; use this as the source of truth for public integration paths.
- [ ] [Introduction](https://docs.keyframelabs.com/) - understand personas, common use cases, and the product promise.
- [ ] [Get an API key](https://docs.keyframelabs.com/guides/getting-started/get-your-api-key) - understand how API keys are created and why secret keys must stay server-side.
- [ ] [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted) - fastest path to a browser demo using `PersonaEmbed` and a publishable key.
- [ ] [Persona library](https://docs.keyframelabs.com/guides/resources/persona-library) - learn what stock personas are available for demos.
- [ ] [[Repositories/monorepo/Definitions/Publishable Key|Publishable Key]] - clarify which key can go in browser code and which key cannot.

### 2. Integration Choice: Widget, Hosted, Or Self Managed

- [ ] [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview) - compare widget, hosted, and self-managed tradeoffs before choosing a demo architecture.
- [ ] [Hosted integration](https://docs.keyframelabs.com/guides/integrate/hosted) - understand the low-code path where Keyframe manages provider/client plumbing.
- [ ] [Self managed integration](https://docs.keyframelabs.com/guides/integrate/self-managed) - understand the custom-control path where your infrastructure keeps provider keys.
- [ ] [[Repositories/monorepo/Workflows/Real-Time Persona Session|Real-Time Persona Session]] - learn the actual session path in this monorepo.
- [ ] [[Repositories/monorepo/Architecture|monorepo Architecture]] - build the mental map of frontend, API, Supabase, LiveKit, voice agent, and avatar runtime.

### 3. JavaScript Client Packages For Demos

- [ ] [Keyframe JavaScript SDK docs](https://docs.keyframelabs.com/sdk-reference/javascript) - learn the three layers: SDK, Elements, and React.
- [ ] [[Repositories/monorepo/Structure/js-react/js-react|js-react]] - preferred read for React demo apps because it exposes Elements as React components/props.
- [ ] [[Repositories/monorepo/Structure/js-elements/js-elements|js-elements]] - read to understand `PersonaEmbed`, `PersonaView`, custom elements, and browser media behavior.
- [ ] [[Repositories/monorepo/Structure/js-sdk/js-sdk|js-sdk]] - read after Elements/React when you need lower-level session control.
- [ ] [[Repositories/monorepo/Definitions/PersonaSession|PersonaSession]] - understand the live session object and what it controls.
- [ ] [[Repositories/monorepo/Definitions/createClient|createClient]] - understand the lower-level SDK entry point.

### 4. Existing Demo And Platform Code

- [ ] [[Repositories/monorepo/Structure/demo/demo|demo app]] - read the current Vite/React demo implementation and its backend calls.
- [ ] [[Repositories/monorepo/Workflows/Demo Scoring And Share Flow|Demo Scoring And Share Flow]] - useful for interview/simulation style demos.
- [ ] [[Repositories/monorepo/Structure/platform/platform|platform app]] - read when you need dashboard, embed config, API key, or account-management behavior.
- [ ] [[Repositories/monorepo/Workflows/Embed Config Builder Lifecycle|Embed Config Builder Lifecycle]] - learn how publishable embed configs are created and used.
- [ ] [[Repositories/monorepo/Concepts/Embed Config|Embed Config]] - read before building configurable demos.

### 5. Session API, Voices, And Model Catalogs

- [ ] [Create session API](https://docs.keyframelabs.com/api-reference/sessions/create-session) - learn the server-side session creation contract.
- [ ] [List voices API](https://docs.keyframelabs.com/api-reference/voices/list-voices) - learn how demos can offer voice selection.
- [ ] [List LLM models API](https://docs.keyframelabs.com/api-reference/llm-models/list-llm-models) - learn how demos can offer model selection.
- [ ] [[Repositories/monorepo/Structure/modal_kflapi/modal_kflapi|modal_kflapi]] - read when a demo depends on backend session creation or auth.
- [ ] [[Repositories/monorepo/Workflows/API Key And Auth Flow|API Key And Auth Flow]] - understand browser-safe vs server-only trust boundaries.
- [ ] [[Repositories/monorepo/Database Tables/Embed Configs Table|Embed Configs Table]] - read when debugging publishable keys or allowed origins.

### 6. Agent And Voice Provider Paths

- [ ] [OpenAI Realtime hosted guide](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/openai-realtime) - learn hosted OpenAI Realtime integration.
- [ ] [OpenAI Realtime self-managed guide](https://docs.keyframelabs.com/guides/self-managed/real-time-llms/openai-realtime) - learn self-managed OpenAI Realtime integration.
- [ ] [ElevenLabs hosted guide](https://docs.keyframelabs.com/guides/hosted/agent-frameworks/elevenlabs-agents) - learn hosted ElevenLabs agent integration.
- [ ] [ElevenLabs self-managed guide](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/elevenlabs-agents) - learn self-managed ElevenLabs agent integration.
- [ ] [[Repositories/monorepo/Concepts/Voice AI Pipeline|Voice AI Pipeline]] - understand STT -> LLM -> TTS -> avatar video.
- [ ] [[Repositories/monorepo/Structure/kfl-voice-agent/kfl-voice-agent|kfl-voice-agent]] - read when using Keyframe's first-party voice-agent runtime.

### 7. Meeting Demo Path

- [ ] [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams) - learn the Zoom/Meet/Teams use case.
- [ ] [Create meet bot API](https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot) - learn required meeting bot request fields.
- [ ] [Get meet bot status API](https://docs.keyframelabs.com/api-reference/meet-bots/get-meet-bot-status) - learn how to observe bot state.
- [ ] [Stop meet bot API](https://docs.keyframelabs.com/api-reference/meet-bots/stop-meet-bot) - learn cleanup behavior.
- [ ] [[Repositories/monorepo/Structure/meet-bot/meet-bot|meet-bot]] - read the local static page that renders the persona camera feed for Recall.ai.
- [ ] [[Repositories/monorepo/Technologies/Recall.ai|Recall.ai]] - understand the meeting-bot dependency.

### 8. LiveKit And Realtime Media

- [ ] [Keyframe LiveKit Agents guide](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents) - learn the Keyframe avatar plugin path for LiveKit Agents.
- [ ] [LiveKit docs](https://docs.livekit.io/home/) - understand rooms, participants, media tracks, and realtime transport.
- [ ] [LiveKit Agents docs](https://docs.livekit.io/agents/) - understand server-side AI participants.
- [ ] [MDN WebRTC API](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API) - learn browser media and peer connection fundamentals.
- [ ] [[Repositories/monorepo/Concepts/LiveKit Transport|LiveKit Transport]] - connect LiveKit concepts back to this repo.
- [ ] [[Repositories/monorepo/Structure/livekit-plugins-keyframe/livekit-plugins-keyframe|livekit-plugins-keyframe]] - read the Python plugin used by LiveKit Agents.

### 9. Frontend App Stack

- [ ] [React Learn](https://react.dev/learn) - learn the component/state model used by the demo and platform apps.
- [ ] [Vite Guide](https://vite.dev/guide/) - learn the dev server and build tool behind the React apps.
- [ ] [TypeScript Handbook](https://www.typescriptlang.org/docs/) - learn types, interfaces, narrowing, and module patterns.
- [ ] [pnpm workspaces](https://pnpm.io/workspaces) - understand how the repo links packages and apps together.
- [ ] [[Repositories/monorepo/Technologies/React|React]] - repo-specific notes.
- [ ] [[Repositories/monorepo/Technologies/Vite|Vite]] - repo-specific notes.
- [ ] [[Repositories/monorepo/Technologies/TypeScript|TypeScript]] - repo-specific notes.
- [ ] [[Repositories/monorepo/Technologies/pnpm Workspaces|pnpm Workspaces]] - repo-specific notes.

### 10. Local Development Essentials

- [ ] [[Repositories/monorepo/Workflows/Local Development And Docker|Local Development And Docker]] - learn which pieces are laptop-friendly and which need hosted providers/GPU capacity.
- [ ] [[Repositories/monorepo/Concepts/Local HTTPS And Cloudflared Tunnels|Local HTTPS And Cloudflared Tunnels]] - learn why public HTTPS matters for iOS, OAuth redirects, webhooks, and secure browser APIs.
- [ ] [[Repositories/monorepo/Workflows/Cloudflared Tunnel Development|Cloudflared Tunnel Development]] - learn ingress, hostname, service, and tunnel routing.
- [ ] [[Repositories/monorepo/Workflows/Local Supabase Development And Seeding|Local Supabase Development And Seeding]] - learn local database startup, migrations, and seed data.
- [ ] [[Repositories/monorepo/Concepts/Secret Management And Environment Variables|Secret Management And Environment Variables]] - avoid leaking keys while building demos.

### 11. Backend, Data, And API Knowledge

- [ ] [FastAPI docs](https://fastapi.tiangolo.com/) - useful when creating or changing demo APIs.
- [ ] [Supabase local development](https://supabase.com/docs/guides/local-development/cli/getting-started) - understand local Supabase.
- [ ] [Supabase migrations](https://supabase.com/docs/guides/deployment/database-migrations) - understand schema change workflow.
- [ ] [[Repositories/monorepo/Structure/modal_kfldemoapi/modal_kfldemoapi|modal_kfldemoapi]] - read when building simulation/interview style demos.
- [ ] [[Repositories/monorepo/Concepts/Database Migrations And Seeding|Database Migrations And Seeding]] - repo-specific mental model.
- [ ] [[Repositories/monorepo/Definitions/Database RPC|Database RPC]] - learn why atomic database operations matter.

### 12. Testing And Demo Quality

- [ ] [[Repositories/monorepo/Concepts/Testing Taxonomy|Testing Taxonomy]] - learn unit, integration, E2E, smoke, and regression tests.
- [ ] [[Repositories/monorepo/Definitions/Integration Test|Integration Test]] - use when a demo crosses frontend/API/provider boundaries.
- [ ] [[Repositories/monorepo/Definitions/Regression Test|Regression Test]] - use when fixing a demo bug so it does not return.
- [ ] [pytest docs](https://docs.pytest.org/en/stable/) - useful for API/integration tests.
- [ ] [Playwright docs](https://playwright.dev/docs/intro) - useful for browser demo checks.
- [ ] [[Repositories/monorepo/Structure/modal_kflapi_tests/modal_kflapi_tests|modal_kflapi_tests]] - read the existing API test harness.

### 13. Deployment And Operations

- [ ] [Cloudflare Workers Wrangler](https://developers.cloudflare.com/workers/wrangler/) - learn deployment for Vite/static surfaces using Wrangler.
- [ ] [Cloudflare Tunnel docs](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) - learn public HTTPS tunnels for local demos.
- [ ] [Modal docs](https://modal.com/docs) - understand deployed Python API surfaces in the repo.
- [ ] [Terraform intro](https://developer.hashicorp.com/terraform/intro) - learn infrastructure-as-code basics.
- [ ] [[Repositories/monorepo/Structure/terraform/terraform|terraform]] - repo-specific infrastructure notes.
- [ ] [[Repositories/monorepo/Technologies/Secret Managers|Secret Managers]] - choose safe places for production keys.

### 14. Deeper Media And ML Internals

- [ ] [[Repositories/monorepo/Structure/ml/windy-inference/windy-inference|windy-inference]] - read only after you understand demo-level session flow.
- [ ] [[Repositories/monorepo/Structure/ml/InfiniteTalk/InfiniteTalk|InfiniteTalk]] - useful for queued video generation demos, not first for live avatar demos.
- [ ] [[Repositories/monorepo/Technologies/GPU Inference|GPU Inference]] - learn the runtime constraints behind avatar rendering.
- [ ] [[Repositories/monorepo/Structure/remotion-mono/remotion-mono|remotion-mono]] - read for rendered-video/share-card style demos.
- [ ] [Remotion docs](https://www.remotion.dev/docs/) - useful when demos need generated video assets or share outputs.

## Demo Use Cases To Keep In Mind

- [ ] Customer support agent with a trusted human-facing persona.
- [ ] Learning coach or tutoring practice session.
- [ ] Interview or sales roleplay simulator.
- [ ] Healthcare or benefits explainer with careful disclaimers.
- [ ] Meeting assistant that joins Zoom, Google Meet, or Teams.
- [ ] Internal training avatar with custom voice/model/persona choices.
- [ ] LiveKit-based self-managed voice agent demo.

## Related Topics

- [[Keyframe Labs Docs Study Guide - Demo Applications]]
- [[Keyframe Labs Docs Reading Map]]
- [[Study Plan - Keyframe Avatar Demo Apps]]
- [[Repositories/monorepo/monorepo|Keyframe Labs monorepo]]
- [[Repositories/monorepo/Workflows/Product Learning Path|Product Learning Path]]
- [[Repositories/monorepo/Workflows/Test-Driven Reading Path|Test-Driven Reading Path]]
- [[Repositories/monorepo/Definitions/Onboarding Glossary|Onboarding Glossary]]

## Source Notes

- Keyframe docs were checked on 2026-06-29.
- This list favors material that helps build visible, working demo apps before backend, infrastructure, or ML internals.
