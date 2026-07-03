---
type: workflow
repo: monorepo
status: growing
lifecycle_stage: testing
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/workflow
  - pkm/domain/testing
---

# Test-Driven Reading Path

## Objective
Use tests as behavioral documentation to learn intended edge cases before reading implementation details.

## Description
### What
The tests describe what the repo promises under failure: no healthy nodes, full capacity, org concurrency, invalid origins, bad provider keys, immutable active configs, draft publish constraints, billing blockers, and voice-agent interruption behavior.

### Why
Runtime code can be noisy because it handles networked providers and cloud state. Tests are the cleanest statements of expected behavior and should be read before implementation when learning a subsystem.

### When
Use this workflow before modifying any code path with auth, billing, sessions, embed configs, reservations, or realtime turn handling.

### How
Start with test names, then fixtures, then assertions, then implementation. For each assertion, write the product rule in plain language and link it back to the note that owns the concept.

## Scope
| Test Area | What It Teaches | Primary Notes |
| --- | --- | --- |
| Session lifecycle | Capacity, dispatch cleanup, quarantined/expired nodes, org concurrency | [[Workflows/Real-Time Persona Session]], [[Concepts/Reservation And Node Capacity]] |
| Embed session | Publishable key lookup, origin validation, provider-token failures | [[Concepts/Publishable Key And Secret Key]], [[Concepts/Embed Config]] |
| Embed drafts | Draft mutability, publish requirements, deactivate/reactivate | [[Workflows/Embed Config Builder Lifecycle]] |
| Billing | Plan limits, blocked subscriptions, webhook idempotency, meter reporting | [[Workflows/Billing Usage And Session Limits]] |
| KFL voice agent | Endpoint coalescing, interruption phases, TTS behavior | [[Workflows/KFL Voice Agent Session]] |

## Use Case
Use when you need confidence about expected behavior before changing code, or when implementation and docs disagree.

## Stage In Lifecycle
Testing and discovery. It is a learning method that uses tests as source evidence.

## Importance
High. Tests encode the repo’s most important negative paths.

## Risks
- Tests may skip provider integration when secrets are absent, so skipped tests still teach expected integration boundaries.
- Some tests seed state manually; distinguish product setup from test setup.
- Test comments can lag implementation, so assertions and fixtures are stronger than prose comments.
- A missing test around a boundary is itself a research question for [[Questions]].

## Input / Output
| Input | Output |
| --- | --- |
| Test file or failure mode | Product rule, owning implementation path, and related concept note |
| Planned code change | Minimal set of behavior tests to read or extend |

## Resources
- [[Philosophies/Test-Driven Code Reading]]
- [[Workflows/Product Learning Path]]
- [[Concepts/Research Source Index]]
- Pytest docs: https://docs.pytest.org/
- Go testing package docs: https://pkg.go.dev/testing

## Related Topics
- [[Workflows/Real-Time Persona Session]]
- [[Workflows/Embed Config Builder Lifecycle]]
- [[Workflows/Billing Usage And Session Limits]]
- [[Workflows/KFL Voice Agent Session]]

## Evidence
- modal_kflapi_tests/test_session_lifecycle.py
- modal_kflapi_tests/test_embed_session.py
- modal_kflapi_tests/test_embed_config_drafts.py
- modal_kflapi_tests/test_embed_config_editing.py
- modal_kflapi_tests/test_billing_usage.py
- modal_kflapi_tests/test_billing_blocking.py
- modal_kflapi_tests/test_billing_webhooks.py
- kfl-voice-agent/internal/voiceagent/session_interrupt_test.go
- kfl-voice-agent/internal/voiceagent/*_test.go
