---
type: concept
repo: monorepo
status: growing
lifecycle_stage: testing
importance: high
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/testing
---

# Testing Taxonomy

## Objective
Define common test types and map them to the monorepo's current tests.

## Description
Testing terms describe how much of the system is under test. Unit tests isolate small logic. Integration tests check multiple real pieces together. End-to-end tests exercise full user/system flows. Regression tests protect known fixed bugs. Smoke tests check basic startup/health.

## Scope
| Test Type | Definition | Repo Examples |
| --- | --- | --- |
| Unit test | One function/class/module in isolation | Provider parsing/model validation tests |
| Integration test | Multiple real components together | `modal_kflapi_tests` using API URL + Supabase env |
| End-to-end test | Full user-facing flow, often browser/API/provider | Stripe E2E-style tests when enabled |
| Regression test | Prevents a known bug from returning | Permission, concurrency, webhook replay, interruption tests |
| Smoke test | Quick check that a service basically starts/responds | Health/route checks when present |

## Use Case
Use this concept when deciding how much environment a test needs, how to name a test, or how to read test coverage as behavioral documentation.

## Stage In Lifecycle
Testing.

## Importance
High. Tests are one of the best sources of product intent in this repo.

## Risks
- Integration tests may mutate real Supabase/Stripe/provider state if env files point at shared systems.
- Regression tests only help if run before merge/deploy.
- Unit tests can miss cross-service contract bugs.
- End-to-end tests can become flaky if external providers are unstable.

## Input / Output
| Input | Output |
| --- | --- |
| Code behavior and test harness | Confidence signal at a specific boundary level |

## Resources
- [[Definitions/Integration Test]]
- [[Definitions/Regression Test]]
- [[Structure/modal_kflapi_tests/modal_kflapi_tests]]
- [[Workflows/Test-Driven Reading Path]]

## Related Topics
- [[Definitions/Race Condition]]
- [[Concepts/Production Database Safety]]

## Evidence
- pyproject.toml
- modal_kflapi_tests/conftest.py
- modal_kflapi_tests/test_api_key_creation.py
- modal_kflapi_tests/test_billing_stripe_e2e.py
- modal_kflapi_tests/test_concurrency_limits.py
- kfl-voice-agent/internal/voiceagent/session_interrupt_test.go
