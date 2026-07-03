---
type: technology
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: medium
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/technology
  - pkm/domain/integration
---

# Recall.ai

## Objective
Explain Recall.ai's role in the repo's meeting-bot flow.

## Description
Recall.ai is a meeting bot provider used to send a bot into video meetings such as Zoom, Google Meet, or Teams. In this repo, the main API creates a Keyframe avatar session, creates a webpage output URL, and asks Recall.ai to join the meeting with that webpage as the bot's camera output.

## Scope
| Repo Surface                         | Role                                                           |
| ------------------------------------ | -------------------------------------------------------------- |
| `modal_kflapi` `/v1/meet-bot`        | Creates session/reservation and calls Recall bot API           |
| `meet-bot/index.html`                | Browser page that redeems init token and renders `PersonaView` |
| `Recall-Api-Key` header              | Customer/provider credential required by API request           |
| `output_media.camera.kind = webpage` | Tells Recall to use the hosted webpage as camera output        |

## Use Case
Use this note when changing meeting-bot creation, status, stop behavior, or the webpage that renders the avatar inside a video meeting.

## Stage In Lifecycle
Runtime and integration.

## Importance
Medium. It is a specific provider integration rather than the core avatar runtime.

## Risks
- Recall API failures should release reservations to avoid stranded capacity.
- The init token must remain short-lived and encrypted.
- The output webpage depends on reachable API URLs and embed/session credentials.

## Input / Output
| Input                                               | Output                                                     |
| --------------------------------------------------- | ---------------------------------------------------------- |
| Meeting URL, Recall API key, persona/config request | Recall bot joining the meeting with Keyframe avatar output |

## Resources
- [[Structure/meet-bot/meet-bot]]
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Definitions/LiveKit Agent]]
- Recall.ai docs: https://docs.recall.ai/

## Related Topics
- [[Concepts/Voice AI Pipeline]]
- [[Concepts/Publishable Key And Secret Key]]
- [[Workflows/Real-Time Persona Session]]

## Evidence
- modal_kflapi/app.py
- meet-bot/index.html
- meet-bot/package.json
- modal_kflapi_tests/test_meet_bot.py
