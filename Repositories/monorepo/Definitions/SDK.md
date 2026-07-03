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
# SDK

## Objective
The objective of the SDK layer is to expose the repository's live avatar capabilities as reusable frontend package APIs instead of forcing product teams to call low-level transport, session, and UI internals directly.

## Description
In this repo, SDK refers to the public JavaScript package surface centered on [[Structure/js-sdk/js-sdk|js-sdk]] and layered upward through [[Structure/js-elements/js-elements|js-elements]] and [[Structure/js-react/js-react|js-react]].

What it is:

- `js-sdk`: low-level browser session control.
- `js-elements`: Web Components that package common UI behavior.
- `js-react`: React bindings around the lower-level package behavior.

Why it exists:

- Different integrators need different levels of control.
- A small SDK boundary lets backend admission and LiveKit transport stay hidden behind stable frontend APIs.
- Layered packages reduce duplication while keeping product-specific UI optional.

When it is used:

- When an external or internal frontend embeds a persona/avatar experience.
- When a developer wants a stable package contract rather than copying implementation details from demo apps.
- When higher-level UI packages need a shared runtime primitive.

How it works:

1. Backend admission returns session credentials.
2. The low-level SDK turns those credentials into a [[Definitions/PersonaSession|PersonaSession]].
3. Higher-level packages can compose that session object into custom elements or React components.
4. Application code chooses the layer that matches its desired control surface.

## Scope
| In Scope | Out Of Scope |
|---|---|
| Browser package APIs | Central API implementation details |
| Session creation and lifecycle control | Model training or inference internals |
| UI package layering | Infrastructure provisioning |
| Public integration surface | Private worker implementation details |

## Use Case
Use this definition when a note says “SDK” and you need to know whether it means the low-level JS session package, the larger frontend package family, or the public integration boundary.

## Stage In Lifecycle
Runtime and design. The SDK is designed as a package boundary and used during live frontend sessions.

## Importance
Core. The SDK is the public-facing shape of the repo's realtime avatar capability.

## Risks
- The word “SDK” can be ambiguous unless notes name the exact package.
- Public SDK behavior is harder to change than internal demo behavior.
- If README examples drift from backend routes, integrators may copy stale flows.

## Input / Output
| Direction | Value | Meaning |
|---|---|---|
| Input | backend-issued session credentials | Values needed to enter a realtime session |
| Input | app callbacks and UI preferences | Hooks or controls chosen by the integrating frontend |
| Output | session object, custom element, or React component | The integration surface used by the application |

## Resources
- [[Structure/js-sdk/js-sdk]]
- [[Structure/js-elements/js-elements]]
- [[Structure/js-react/js-react]]
- [[Philosophies/Layered SDK Abstraction]]

## Related Topics
- [[Definitions/createClient|createClient()]]
- [[Definitions/PersonaSession|PersonaSession]]
- [[Definitions/Backend Admission|Backend Admission]]
- [[Definitions/Live Avatar Session|Live Avatar Session]]

## Evidence
- `js-sdk/src/index.ts`
- `js-sdk/README.md`
- `js-elements`
- `js-react`
