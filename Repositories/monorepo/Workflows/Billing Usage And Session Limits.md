---
type: workflow
repo: monorepo
status: growing
lifecycle_stage: operations
importance: high
last_reviewed: 2026-06-28
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/workflow
  - pkm/domain/billing
---

# Billing Usage And Session Limits

## Objective
Explain how plan limits, billing eligibility, reservation duration, concurrency, credit calculation, and Stripe meter reporting govern realtime sessions.

## Description
### What
Billing and limits are admission-control and accounting systems wrapped around realtime sessions. A session cannot start unless the org is eligible, the requested model is allowed, concurrency is available, and capacity can be reserved. Usage is later calculated from reservation timing and model rates.

### Why
Realtime avatar sessions consume scarce GPU and provider resources. The repo protects those resources before dispatch and reports usage after the session lifecycle completes. Free-plan usage is checked against included credits; paid-plan usage depends on subscription status and Stripe metering.

### When
Billing checks run before `/v1/sessions`, plugin sessions, KFL sessions, embed sessions, and preview sessions reserve capacity. Reporting runs later through the meter reporter/reaper loop after reservations have `ready_at` and `released_at`.

### How
`_resolve_plan_limits` merges base plan config with plan overrides. `check_billing_eligible` handles free-plan period usage or paid subscription status. `reserve_capacity` enforces org concurrency and node capacity atomically. `_report_unreported_credits` selects completed, unreported reservations, calculates minutes times model credit rate, marks reporting started, reports to Stripe, then marks reported.

## Scope
| Mechanism | Logic | Evidence |
| --- | --- | --- |
| Plan resolution | Base plan plus optional overrides; custom can use override rows | modal_kflapi/app.py |
| Free eligibility | Monthly period anchored to org creation day, credits from reservation history | modal_kflapi/app.py |
| Paid eligibility | Subscription row must exist and not be blocked | modal_kflapi/app.py |
| Concurrency | Active reservations counted by org under advisory transaction lock | reserve_capacity RPC |
| Metering | Ready-to-release minutes multiplied by model credit rate | calculate_org_credits RPC, meter reporter |
| Idempotency | Stripe webhook events stored; meter reporting has started/reported timestamps | modal_kflapi/app.py |

## Use Case
Use when debugging 402, 429, unexpected 503, missing Stripe usage, plan upgrades/downgrades, custom plan overrides, or incorrect credit reporting.

## Stage In Lifecycle
Operations and runtime. It gates runtime start and reconciles usage after runtime end.

## Importance
High. Billing errors either block valid sessions, allow unpaid usage, or corrupt customer trust.

## Risks
- Fail-open billing eligibility protects availability but can permit unpaid usage when dependencies fail.
- Reservation state is the source of metered usage; missing `released_at` or `ready_at` changes billing math.
- Concurrent meter reporters could double-report without lock/started markers.
- Stripe period field changes can break subscription period handling.
- Custom plan overrides must stay aligned with UI assumptions and seed data.

## Input / Output
| Input | Output |
| --- | --- |
| Org plan, created_at, subscription row, overrides | Effective included credits, concurrency, duration, allowed models |
| Session start request | Allowed, payment-required, concurrency-limited, or capacity-limited response |
| Completed reservation | Credits reported to Stripe meter and `meter_reported_at` set |

## Resources
- [[Concepts/Billing Credits And Plans]]
- [[Concepts/Reservation And Node Capacity]]
- [[Technologies/Stripe Billing]]
- [[Technologies/Supabase]]
- Stripe usage-based billing docs: https://docs.stripe.com/billing/subscriptions/usage-based/recording-usage-api
- Stripe webhooks docs: https://docs.stripe.com/webhooks

## Related Topics
- [[Workflows/Real-Time Persona Session]]
- [[Philosophies/Reservation-Based GPU Capacity]]
- [[Structure/modal_kflapi/modal_kflapi]]
- [[Structure/supabase-kfl/supabase/supabase]]

## Evidence
- modal_kflapi/app.py
- supabase-kfl/supabase/migrations/20260210120000_billing.sql
- supabase-kfl/supabase/migrations/20260507150000_plan_overrides_credit_grant_mode.sql
- supabase-kfl/supabase/data/plan.ts
- modal_kflapi_tests/test_billing_usage.py
- modal_kflapi_tests/test_billing_blocking.py
- modal_kflapi_tests/test_billing_webhooks.py
- modal_kflapi_tests/test_concurrency_limits.py
