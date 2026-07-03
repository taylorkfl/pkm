---
type: concept
repo: monorepo
status: growing
lifecycle_stage: operations
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/concept
  - pkm/domain/billing
---

# Billing Credits And Plans

## Objective
Define how plans, credits, model rates, subscription status, and reservation timing combine into product access and usage reporting.

## Description
### What
Billing credits are a usage abstraction over realtime session duration. Plans define included credits, allowed models, max concurrent sessions, and max session duration. Model credit rates convert reservation minutes into billable credits.

### Why
Different avatar/model combinations may cost different amounts, and realtime capacity must be managed before sessions begin. Credits make usage visible and billable while plan limits protect service quality.

### When
Plan checks happen before session reservation. Credit calculation happens after sessions finish, when reservation timing is known.

### How
Free-plan eligibility calculates current period usage from `calculate_org_credits`. Paid-plan eligibility checks subscription status. Meter reporting later finds completed reservations, uses model rates, and reports usage to Stripe.

## Scope
| Concept | Meaning |
| --- | --- |
| Included credits | Period allowance before blocking/free-plan upgrade prompt |
| Allowed models | Plan gate before reserving capacity |
| Max concurrent sessions | Org-wide active reservation limit |
| Max duration seconds | Room/session lifetime bound |
| Credit rate | Credits per minute by model id |
| Grant mode | Whether credits are automatic or custom/prepay managed |

## Use Case
Use when reading billing UI/API behavior, plan seed data, reservation metering, or Stripe integration.

## Stage In Lifecycle
Operations. Billing is checked during runtime admission and reconciled after runtime completion.

## Importance
High. It directly affects customer access and revenue accounting.

## Risks
- Incorrect period calculations can block or over-allow usage.
- Missing model rates fallback can hide pricing errors.
- Custom/prepay plans may not fit simple subscription assumptions.
- Stripe reporting should be idempotent across retries and process crashes.

## Input / Output
| Input | Output |
| --- | --- |
| Org plan and overrides | Effective plan limits |
| Completed reservation timing and model id | Credit usage amount |
| Stripe webhook event | Subscription and grant state updates |

## Resources
- [[Workflows/Billing Usage And Session Limits]]
- [[Technologies/Stripe Billing]]
- [[Concepts/Reservation And Node Capacity]]
- Stripe usage-based billing docs: https://docs.stripe.com/billing/subscriptions/usage-based/recording-usage-api

## Related Topics
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Structure/supabase-kfl/supabase/supabase]]
- [[Philosophies/Reservation-Based GPU Capacity]]

## Evidence
- modal_kflapi/app.py
- supabase-kfl/supabase/migrations/20260210120000_billing.sql
- supabase-kfl/supabase/data/plan.ts
- supabase-kfl/supabase/data/seeds/model_credit_rates.base.json
- modal_kflapi_tests/test_billing_usage.py
- modal_kflapi_tests/test_billing_custom_setup.py
- modal_kflapi_tests/test_billing_plan_changes.py
