---
type: definition
repo: monorepo
status: growing
lifecycle_stage: testing
importance: medium
last_reviewed: 2026-06-29
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/testing
---

# Integration Test

## Objective
Define integration tests in the repo and distinguish them from unit and end-to-end tests.

## Description
An integration test checks multiple real components working together. In this repo, many `modal_kflapi_tests` tests use `httpx` against a configured API URL plus Supabase credentials and fixtures. That makes them integration-style tests even when the target URL is local rather than deployed Modal.

## Scope
| Included | Excluded |
| --- | --- |
| API + Supabase + auth/billing/session behavior | Pure function-only tests |
| Tests requiring env files, external URLs, or seeded data | Browser UI automation by itself |
| Local or deployed API targets through `--url` | Modal-only deployment verification |

## Use Case
Use this note when deciding whether a test needs local services, real credentials, seeded database state, or a deployed/staging API.

## Stage In Lifecycle
Testing.

## Importance
Medium. Integration tests document real contracts across modules and services.

## Risks
- Integration tests can be slower and more environment-sensitive than unit tests.
- Test env files can accidentally point to production if not reviewed.
- Some provider-dependent tests skip or fail when external credentials are missing.

## Input / Output
| Input | Output |
| --- | --- |
| Running API URL, env file, fixtures | Test pass/fail over multi-component behavior |

## Resources
- [[Concepts/Testing Taxonomy]]
- [[Structure/modal_kflapi_tests/modal_kflapi_tests]]
- [[Workflows/Test-Driven Reading Path]]

## Related Topics
- [[Definitions/Regression Test]]
- [[Concepts/Production Database Safety]]

## Evidence
- modal_kflapi_tests/conftest.py
- modal_kflapi_tests/test_api_key_creation.py
- modal_kflapi_tests/test_billing_webhooks.py
- modal_kflapi_tests/test_embed_session.py
- pyproject.toml
