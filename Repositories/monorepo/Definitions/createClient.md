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
# createClient()

## Objective
The objective of `createClient()` is to give browser consumers a small, stable factory for constructing a [[Definitions/PersonaSession|PersonaSession]] from server-minted session credentials without exposing the class constructor as the only entrypoint.

## Description
`createClient()` is the lowest-friction public entrypoint of [[Structure/js-sdk/js-sdk|js-sdk]]. It accepts `PersonaSessionOptions` and immediately returns a new `PersonaSession` instance.

What it is:

- A package-level factory exported by `js-sdk/src/index.ts`.
- A convenience wrapper around `new PersonaSession(options)`.
- The handoff point where [[Definitions/Backend Admission|backend admission]] output becomes a browser-side session object.

Why it exists:

- It gives app developers a memorable SDK verb instead of asking them to instantiate an internal-looking class directly.
- It preserves room for future construction logic, validation, telemetry, defaults, or compatibility shims without changing the public call site.
- It matches the layered package story: higher-level UI wrappers can depend on a stable low-level SDK creation function.

When it is used:

- After a backend endpoint creates a session and returns `serverUrl`, `participantToken`, and `agentIdentity`.
- Before the browser calls `session.connect()`.
- Inside custom frontend integrations that want direct control instead of using `js-elements` or `js-react`.

How it works:

1. The caller obtains `PersonaSessionOptions` from an application backend.
2. The caller passes those options into `createClient(options)`.
3. `createClient()` constructs `PersonaSession` with the same options.
4. The returned object owns the later LiveKit connection, audio streaming, interruption, emotion, and cleanup methods.

## Scope
| In Scope | Out Of Scope |
|---|---|
| Creating a browser-side `PersonaSession` object | Creating a LiveKit room on the server |
| Preserving a stable SDK entrypoint | Authenticating users or enforcing billing |
| Passing `PersonaSessionOptions` through to the session class | Connecting to LiveKit by itself |
| Supporting direct app integration | Owning UI rendering |

## Use Case
A frontend developer calls `createClient()` when they have already received session credentials from the backend and want precise control over connection lifecycle, audio input, callbacks, and session teardown.

## Stage In Lifecycle
Runtime. It runs after server admission and before the browser joins the realtime room.

## Importance
Core. `createClient()` is the smallest public API surface that turns backend session admission into a programmable browser session. If this boundary is confusing, the rest of the JS SDK usage model becomes harder to reason about.

## Risks
- Because it is currently a thin wrapper, future changes must preserve its simple construction semantics or document the new side effects clearly.
- The SDK README can drift from backend route names; for example, examples should stay aligned with the canonical session creation route.
- Callers may incorrectly treat `createClient()` as a network call. The network connection begins when `PersonaSession.connect()` is invoked.

## Input / Output
| Direction | Value | Meaning |
|---|---|---|
| Input | `PersonaSessionOptions.serverUrl` | LiveKit server URL returned by backend admission |
| Input | `PersonaSessionOptions.participantToken` | LiveKit participant token minted by backend admission |
| Input | `PersonaSessionOptions.agentIdentity` | Target avatar or agent participant identity |
| Input | callback functions | Browser hooks for track, state, close, and agent-state events |
| Output | `PersonaSession` | Browser-side object that controls one live avatar session |

## Resources
- [[Structure/js-sdk/js-sdk]]
- [[Definitions/PersonaSession|PersonaSession]]
- [[Definitions/Backend Admission|Backend Admission]]
- [[Definitions/SDK|SDK]]

## Related Topics
- [[Definitions/Live Avatar Session|Live Avatar Session]]
- [[Definitions/LiveKit Room|LiveKit Room]]
- [[Concepts/LiveKit Transport]]
- [[Philosophies/Layered SDK Abstraction]]

## Evidence
- `js-sdk/src/index.ts:24`
- `js-sdk/src/index.ts:33`
- `js-sdk/src/types.ts:27`
- `js-sdk/README.md`
