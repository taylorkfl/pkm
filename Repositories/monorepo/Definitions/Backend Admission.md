---
type: definition
repo: monorepo
status: growing
lifecycle_stage: runtime
importance: core
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/definition
---
# Backend Admission

## Objective
The objective of backend admission is to decide whether a browser is allowed to start a live avatar session and, if allowed, return the LiveKit credentials and routing information needed by the frontend SDK.

## Description
Backend admission is the server-side gate before [[Definitions/createClient|createClient()]] and [[Definitions/PersonaSession|PersonaSession]] can do useful work. It converts an authenticated product or embed request into a session allocation.

What it is:

- A server-controlled decision point in the central API.
- The place where authentication, persona lookup, model/provider selection, billing/session limits, capacity reservation, LiveKit room creation, token minting, and worker dispatch can be coordinated.
- The producer of the `serverUrl`, `participantToken`, and `agentIdentity` consumed by the JS SDK.

Why it exists:

- The browser cannot be trusted to create privileged LiveKit sessions directly.
- Live avatar sessions consume scarce runtime capacity and may have billing implications.
- Admission needs a single place to align product policy with realtime infrastructure setup.

When it is used:

- When a product frontend starts a persona session.
- When an embeddable client creates a session through publishable-key flow.
- Before the browser attempts to connect to LiveKit.

How it works at a high level:

1. A frontend or backend caller requests a session from the central API.
2. The API validates the caller and target persona.
3. The API checks usage limits, billing state, and capacity constraints.
4. The API creates a LiveKit room/token pair and dispatches or reserves worker capacity.
5. The API returns connection details to the caller.
6. The browser passes those details into the SDK and connects.

## Scope
| In Scope | Out Of Scope |
|---|---|
| Session authorization and setup | Browser UI rendering |
| Billing/session-limit checks | Direct avatar audio playback |
| LiveKit room and token creation | Client-side byte-stream writes |
| Worker dispatch or reservation handoff | Model internals after the worker starts |
| Embed-session creation policy | Long-term research analysis |

## Use Case
Read this definition when tracing how a user action becomes a usable SDK session and why the JS SDK expects credentials instead of creating sessions by itself.

## Stage In Lifecycle
Runtime. It happens between product intent and browser connection.

## Importance
Core. Backend admission protects the system from unauthorized usage, over-capacity sessions, and disconnected frontend/runtime assumptions.

## Risks
- If admission succeeds without available worker capacity, the frontend can connect to a room with no useful avatar participant.
- If billing checks are bypassed or duplicated inconsistently, product behavior can diverge across API paths.
- If returned credential names drift from SDK expectations, integrations fail at runtime.
- Publishable-key embed flows require especially careful scoping because credentials start in less-trusted browser contexts.

## Input / Output
| Direction | Value | Meaning |
|---|---|---|
| Input | authenticated user, API key, or publishable key | Caller identity and authorization context |
| Input | persona/model/session request | Desired live avatar configuration |
| Input | account limits and billing state | Product constraints on whether a session may start |
| Output | `server_url` | LiveKit endpoint for browser connection |
| Output | `participant_token` | Browser participant credential |
| Output | `agent_identity` | Participant identity targeted by SDK streams and commands |
| Output | session metadata | Identifiers and state used by product/backend workflows |

## Resources
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Workflows/Real-Time Persona Session]]
- [[Definitions/LiveKit Room|LiveKit Room]]
- [[Definitions/Live Avatar Session|Live Avatar Session]]

## Related Topics
- [[Philosophies/Publishable-Key Embed Model]]
- [[Philosophies/Reservation-Based GPU Capacity]]
- [[Structure/supabase-kfl/supabase/supabase|Supabase Contracts]]
- [[Definitions/SDK|SDK]]

## Evidence
- `modal_kflapi/app.py`
- `js-sdk/src/types.ts:27`
- `js-sdk/README.md`
