---
type: study-plan
status: growing
last_reviewed: 2026-06-29
tags:
  - study
  - keyframelabs
  - avatars
  - demo-apps
---

# Study Plan - Keyframe Avatar Demo Apps

## Objective

Build Keyframe Labs product, frontend, API, realtime, and local-development understanding to create polished demo applications that show avatars in multiple use cases.

Primary reading companion: [[Reading List - Keyframe Avatar Demo Apps]]

Docs-focused companion: [[Keyframe Labs Docs Study Guide - Demo Applications]]

## Outcome

By the end of this plan, I should be able to:

- [ ] Explain when to use hosted, self-managed, React, Elements, or low-level SDK integration.
- [ ] Build a minimal browser demo with a Keyframe persona.
- [ ] Build a React demo with good state handling, loading states, errors, and session cleanup.
- [ ] Configure persona, voice, LLM, system prompt, greeting, and allowed origins safely.
- [ ] Run or reason through local development with HTTPS, CORS, secrets, and Supabase boundaries.
- [ ] Create at least one use-case demo beyond generic chat.
- [ ] Explain when LiveKit, Recall.ai, provider APIs, or backend endpoints are required.

## Phase 0 - Access And Safety

Goal: get the credentials and mental guardrails needed before building anything.

- [ ] Read [Keyframe Labs docs](https://docs.keyframelabs.com/).
- [ ] Read [Get an API key](https://docs.keyframelabs.com/guides/getting-started/get-your-api-key).
- [ ] Read [[Repositories/monorepo/Definitions/Publishable Key|Publishable Key]].
- [ ] Read [[Repositories/monorepo/Workflows/API Key And Auth Flow|API Key And Auth Flow]].
- [ ] Confirm what keys are available for local development.
- [ ] Confirm which domain/origin should be used for local browser demos.
- [ ] Record where secrets should live locally and in deployment.
- [ ] Never place a secret API key in frontend code.

Deliverable:

- [ ] A short private note listing needed non-secret account resources: Keyframe account, publishable key, secret API key storage location, allowed origin, optional provider accounts.

## Phase 1 - Fastest Working Persona Demo

Goal: create the smallest possible working browser demo before touching deeper repo internals.

- [ ] Read [Quickstart - hosted](https://docs.keyframelabs.com/guides/getting-started/quickstart-hosted).
- [ ] Read [Integration overview](https://docs.keyframelabs.com/guides/integrate/overview).
- [ ] Read [Keyframe JavaScript SDK docs](https://docs.keyframelabs.com/sdk-reference/javascript).
- [ ] Build or run a minimal HTML demo using `PersonaEmbed`.
- [ ] Test start, stop, microphone permission, disconnect, and refresh behavior.
- [ ] Write down what the publishable key, allowed origin, and `PersonaEmbed.connect()` each do.
- [ ] Capture any errors and map them to likely causes: origin mismatch, missing mic permission, invalid key, provider issue, network issue.

Deliverable:

- [ ] Minimal hosted persona demo that runs locally and can be explained end-to-end.

## Phase 2 - React Demo App Foundation

Goal: move from a raw quickstart into a reusable React demo app.

- [ ] Read [[Repositories/monorepo/Structure/js-react/js-react|js-react]].
- [ ] Read [[Repositories/monorepo/Structure/js-elements/js-elements|js-elements]].
- [ ] Read [[Repositories/monorepo/Structure/demo/demo|demo app]].
- [ ] Review [React Learn](https://react.dev/learn) for components, state, effects, and forms.
- [ ] Review [Vite Guide](https://vite.dev/guide/) for local dev and build commands.
- [ ] Create a Vite React demo shell or adapt the existing `demo` app.
- [ ] Add controls for start, stop, mute, selected use case, and error retry.
- [ ] Add UI states for idle, connecting, connected, speaking/listening, disconnected, and error.
- [ ] Ensure session cleanup runs when leaving the page or stopping the demo.

Deliverable:

- [ ] React avatar demo foundation ready for multiple use cases.

## Phase 3 - Use-Case Demo Design

Goal: turn the generic avatar into demos that show why the avatar matters.

- [ ] Read [[Repositories/monorepo/Concepts/Persona|Persona]].
- [ ] Read [[Repositories/monorepo/Concepts/Dynamic Variables|Dynamic Variables]].
- [ ] Read [[Repositories/monorepo/Concepts/Per-Session Context|Per-Session Context]].
- [ ] Read [[Repositories/monorepo/Concepts/Emotion Conditioning|Emotion Conditioning]].
- [ ] Choose 3 demo scenarios.
- [ ] Define the user, goal, opening prompt, success moment, and failure risk for each scenario.
- [ ] Add scenario-specific system prompts or context.
- [ ] Add visible scenario controls without exposing implementation details in the UI.
- [ ] Test whether the avatar behavior changes meaningfully across scenarios.

Suggested scenarios:

- [ ] Customer support triage.
- [ ] Interview practice or sales roleplay.
- [ ] Learning coach or tutor.
- [ ] Meeting assistant.
- [ ] Benefits or policy explainer.

Deliverable:

- [ ] Multi-scenario demo app where each mode feels intentionally different.

## Phase 4 - Sessions, Voices, Models, And Backend Boundaries

Goal: understand when frontend-only is enough and when a backend is required.

- [ ] Read [Create session API](https://docs.keyframelabs.com/api-reference/sessions/create-session).
- [ ] Read [List voices API](https://docs.keyframelabs.com/api-reference/voices/list-voices).
- [ ] Read [List LLM models API](https://docs.keyframelabs.com/api-reference/llm-models/list-llm-models).
- [ ] Read [[Repositories/monorepo/Structure/modal_kflapi/modal_kflapi|modal_kflapi]].
- [ ] Read [[Repositories/monorepo/Concepts/Publishable Key And Secret Key|Publishable Key And Secret Key]].
- [ ] Identify which demo actions can be done with a publishable key.
- [ ] Identify which demo actions require a server-held secret key.
- [ ] Add voice/model/persona options only if the required API path is safe.
- [ ] Sketch a small backend endpoint if the demo needs server-side session creation.

Deliverable:

- [ ] Clear boundary diagram or note: browser responsibilities vs backend responsibilities.

## Phase 5 - Provider And Voice Agent Understanding

Goal: understand enough STT, LLM, and TTS flow to customize demos intelligently.

- [ ] Read [[Repositories/monorepo/Concepts/Voice AI Pipeline|Voice AI Pipeline]].
- [ ] Read [Hosted integration](https://docs.keyframelabs.com/guides/integrate/hosted).
- [ ] Read [Self managed integration](https://docs.keyframelabs.com/guides/integrate/self-managed).
- [ ] Read either the OpenAI Realtime docs path or the ElevenLabs docs path from the reading list.
- [ ] Read [[Repositories/monorepo/Structure/kfl-voice-agent/kfl-voice-agent|kfl-voice-agent]] if using the first-party voice agent path.
- [ ] Write a simple explanation of where STT, LLM, and TTS happen in hosted vs self-managed mode.
- [ ] Test one provider-specific customization such as greeting, system prompt, voice, or emotion.

Deliverable:

- [ ] Demo can be configured for at least one provider path and explained without hand-waving.

## Phase 6 - Meeting Bot Demo

Goal: learn the Zoom/Meet/Teams avatar path separately from normal browser embed demos.

- [ ] Read [Meetings guide](https://docs.keyframelabs.com/guides/integrate/meetings-zoom-meet-teams).
- [ ] Read [Create meet bot API](https://docs.keyframelabs.com/api-reference/meet-bots/create-meet-bot).
- [ ] Read [[Repositories/monorepo/Structure/meet-bot/meet-bot|meet-bot]].
- [ ] Read [[Repositories/monorepo/Technologies/Recall.ai|Recall.ai]].
- [ ] Identify required inputs: meeting URL, persona, Recall API key, Keyframe API key, bot name, prompt, greeting, voice, model.
- [ ] Decide whether this demo should use a local tunnel, deployed endpoint, or hosted platform path.
- [ ] Test create, status, and stop behavior in a controlled meeting.
- [ ] Document cleanup behavior when the meeting ends or the bot is removed.

Deliverable:

- [ ] Meeting avatar proof-of-concept with a known cleanup procedure.

## Phase 7 - LiveKit Self-Managed Demo

Goal: understand the advanced path for teams already building on LiveKit Agents.

- [ ] Read [Keyframe LiveKit Agents guide](https://docs.keyframelabs.com/guides/self-managed/agent-frameworks/livekit-agents).
- [ ] Read [[Repositories/monorepo/Definitions/LiveKit Agent|LiveKit Agent]].
- [ ] Read [[Repositories/monorepo/Concepts/LiveKit Transport|LiveKit Transport]].
- [ ] Read [[Repositories/monorepo/Structure/livekit-plugins-keyframe/livekit-plugins-keyframe|livekit-plugins-keyframe]].
- [ ] Review [LiveKit Agents docs](https://docs.livekit.io/agents/).
- [ ] Build or trace a tiny LiveKit agent that starts a Keyframe `AvatarSession`.
- [ ] Test persona slug vs persona ID selection.
- [ ] Test `set_emotion()` if using a compatible persona.

Deliverable:

- [ ] LiveKit self-managed demo or a written architecture plan showing exactly what remains to be built.

## Phase 8 - Local Development And Mobile Testing

Goal: avoid losing time to browser, iOS, redirect, and tunnel issues.

- [ ] Read [[Repositories/monorepo/Workflows/Local Development And Docker|Local Development And Docker]].
- [ ] Read [[Repositories/monorepo/Concepts/Local HTTPS And Cloudflared Tunnels|Local HTTPS And Cloudflared Tunnels]].
- [ ] Read [[Repositories/monorepo/Workflows/Cloudflared Tunnel Development|Cloudflared Tunnel Development]].
- [ ] Read [[Repositories/monorepo/Workflows/Local Supabase Development And Seeding|Local Supabase Development And Seeding]] if the demo touches local auth/database state.
- [ ] Verify local browser demo over `localhost`.
- [ ] Verify iOS device demo through an HTTPS tunnel if needed.
- [ ] Verify allowed origins, CORS settings, auth redirects, and webhook URLs match the environment.

Deliverable:

- [ ] Local development checklist that catches origin/tunnel/secret mistakes before demos.

## Phase 9 - Quality, Testing, And Regression Protection

Goal: make demos stable enough to show repeatedly.

- [ ] Read [[Repositories/monorepo/Concepts/Testing Taxonomy|Testing Taxonomy]].
- [ ] Read [[Repositories/monorepo/Definitions/Regression Test|Regression Test]].
- [ ] Read [[Repositories/monorepo/Definitions/Integration Test|Integration Test]].
- [ ] Add a smoke checklist for each demo: load, connect, speak, interrupt, stop, refresh.
- [ ] Add at least one regression test or scripted check for a bug encountered while building.
- [ ] Use Playwright or manual browser testing for the most important UI flows.
- [ ] Use pytest/API tests only when backend behavior is part of the demo.

Deliverable:

- [ ] Repeatable demo QA checklist plus at least one automated check for the riskiest flow.

## Phase 10 - Deployment And Sharing

Goal: publish demos safely without leaking secrets or depending on laptop-only state.

- [ ] Read [[Repositories/monorepo/Technologies/Cloudflare Workers Wrangler|Cloudflare Workers Wrangler]].
- [ ] Read [[Repositories/monorepo/Technologies/Secret Managers|Secret Managers]].
- [ ] Read [[Repositories/monorepo/Structure/terraform/terraform|terraform]] only if demo infrastructure expands beyond frontend/static deployment.
- [ ] Choose deployment target: static frontend, Cloudflare Worker, Modal API, or internal platform path.
- [ ] Store secrets in the deployment platform secret manager.
- [ ] Confirm production allowed origins and redirect URLs.
- [ ] Confirm demo teardown/stop behavior.
- [ ] Create a short demo runbook: setup, start, expected behavior, reset, troubleshooting.

Deliverable:

- [ ] Shareable demo with deployment notes and no secrets in frontend code or checked-in files.

## Phase 11 - Optional Deep Dive

Goal: only go here after you can build and explain demos.

- [ ] Read [[Repositories/monorepo/Structure/ml/windy-inference/windy-inference|windy-inference]] for live avatar inference internals.
- [ ] Read [[Repositories/monorepo/Structure/ml/InfiniteTalk/InfiniteTalk|InfiniteTalk]] for queued video generation.
- [ ] Read [[Repositories/monorepo/Structure/remotion-mono/remotion-mono|remotion-mono]] for rendered video/share-card output.
- [ ] Read [[Repositories/monorepo/Technologies/GPU Inference|GPU Inference]] for runtime constraints.
- [ ] Decide whether any demo needs ML/runtime changes or whether product-level integration is enough.

Deliverable:

- [ ] Written decision: stay at client/API demo layer, or invest in ML/runtime understanding for a specific demo.

## Weekly Rhythm

### Week 1 - Working Hosted Demo

- [ ] Finish Phases 0-2.
- [ ] Show a minimal hosted persona in the browser.
- [ ] Convert it into a React demo foundation.

### Week 2 - Use Cases And Config

- [ ] Finish Phases 3-5.
- [ ] Build three scenario modes.
- [ ] Add safe configuration for persona, voice, or model if available.

### Week 3 - Advanced Demo Path

- [ ] Choose either Phase 6 meeting bot or Phase 7 LiveKit self-managed.
- [ ] Build one advanced proof-of-concept.
- [ ] Document which provider keys and hosted services are required.

### Week 4 - Polish And Deployment

- [ ] Finish Phases 8-10.
- [ ] Test local, mobile, and deployed behavior.
- [ ] Create a demo runbook and regression checklist.

## Parking Lot

Use this section for questions that come up while studying.

- [ ] Which Keyframe account, project, and allowed origin should my demos use?
- [ ] Which publishable key should be used for local demos?
- [ ] Which demos require a secret API key and therefore a backend?
- [ ] Which provider path should be the default: hosted OpenAI Realtime, hosted ElevenLabs, KFL voice agent, or LiveKit Agents?
- [ ] Which avatar personas are approved for public demos?
- [ ] Which demos should be deployed publicly vs kept internal?

## Related Topics

- [[Keyframe Labs Docs Study Guide - Demo Applications]]
- [[Keyframe Labs Docs Reading Map]]
- [[Reading List - Keyframe Avatar Demo Apps]]
- [[Repositories/monorepo/monorepo|Keyframe Labs monorepo]]
- [[Repositories/monorepo/Workflows/Product Learning Path|Product Learning Path]]
- [[Repositories/monorepo/Definitions/Onboarding Glossary|Onboarding Glossary]]
