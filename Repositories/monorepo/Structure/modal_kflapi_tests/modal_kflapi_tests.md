---
type: structure-note
repo: monorepo
repo_path: modal_kflapi_tests
status: growing
lifecycle_stage: testing
importance: high
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/structure-note
  - pkm/domain/testing
---

# modal_kflapi_tests

## Objective
Explain the test suite for the main Modal API and how it is executed.

## Description
`modal_kflapi_tests` is a pytest suite for the main API behavior. The harness requires `--env-file` and `--url`, loads Supabase credentials from the env file, and points `httpx` at whichever API URL is provided. It is not Modal-only; it can target local or deployed APIs if the URL and environment are correct.

## Scope
| Path | Role |
| --- | --- |
| `modal_kflapi_tests/conftest.py` | pytest options, env loading, API client, Supabase fixtures |
| `modal_kflapi_tests/utils.py` | small shared helpers |
| `modal_kflapi_tests/test_api_key_creation.py` | API key behavior |
| `modal_kflapi_tests/test_billing_webhooks.py` | Stripe webhook behavior |
| `modal_kflapi_tests/test_concurrency_limits.py` | capacity/concurrency behavior |
| `modal_kflapi_tests/test_meet_bot.py` | meeting bot behavior |
| `modal_kflapi_tests/test_embed_session.py` | embed session behavior |

## Use Case
Use this note before running API tests or using tests as behavioral documentation for auth, billing, sessions, embeds, meet-bot, and concurrency.

## Stage In Lifecycle
Testing.

## Importance
High. These tests are one of the clearest maps of intended API contracts.

## Risks
- The `--env-file` can point to local, development, or another environment; review before running write-heavy tests.
- Some tests require provider credentials and may skip/fail without them.
- The configured `--url` determines whether tests hit local or deployed API.

## Input / Output
| Input | Output |
| --- | --- |
| pytest command with env file and URL | API integration/unit-style test results |

## Resources
- [[Concepts/Testing Taxonomy]]
- [[Definitions/Integration Test]]
- [[Definitions/Regression Test]]
- [[Workflows/Test-Driven Reading Path]]

## Related Topics
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Concepts/Production Database Safety]]
- [[Definitions/Race Condition]]

## Evidence
- modal_kflapi_tests/conftest.py
- modal_kflapi_tests/utils.py
- modal_kflapi_tests/test_api_key_creation.py
- modal_kflapi_tests/test_billing_webhooks.py
- modal_kflapi_tests/test_concurrency_limits.py
- modal_kflapi_tests/test_meet_bot.py
- pyproject.toml
