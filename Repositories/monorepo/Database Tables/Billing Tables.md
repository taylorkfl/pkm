---
type: definition
repo: monorepo
status: growing
lifecycle_stage: persistence
importance: core
last_reviewed: 2026-06-28
aliases:
  - subscriptions
  - model_credit_rates
  - plan_overrides
  - stripe_processed_events
tags:
  - code-pkm
  - repo/monorepo
  - pkm/type/definition
  - pkm/domain/data
---

# Billing Tables

## Objective
Explain the database tables that store plan state, metering rates, custom limits, and Stripe webhook idempotency.

## Description
### What
The billing tables include `subscriptions`, `model_credit_rates`, `plan_overrides`, and `stripe_processed_events`. `orgs.stripe_customer_id` and `reservations` meter flags also participate in billing.

### Why
The product needs to decide whether an org can start sessions, how many concurrent sessions it can run, how long sessions may last, which models are allowed, and how completed usage maps to Stripe credits.

### When
Used during session eligibility checks, checkout and subscription webhooks, custom plan setup, free-plan credit checks, and scheduled meter reporting.

### How
Webhook handlers update `subscriptions` and `orgs.plan`, processed Stripe event ids are inserted for idempotency, `plan_overrides` customizes limits, and `model_credit_rates` lets `calculate_org_credits` convert completed reservation minutes into credits.

## Scope
| Table | Meaning |
| --- | --- |
| `subscriptions` | Stripe subscription status and period fields for orgs |
| `model_credit_rates` | Credits per minute by avatar model id |
| `plan_overrides` | Per-org custom limits and credit grant mode |
| `stripe_processed_events` | Processed webhook event ids for idempotency |

## Use Case
Read before changing plan limits, Stripe webhook handling, checkout, custom plans, meter reporting, or usage display.

## Stage In Lifecycle
Persistence, billing, and operations.

## Importance
Core. These tables decide eligibility and revenue-impacting usage.

## Risks
- Duplicate or missing Stripe webhook processing can misstate billing.
- Editing meter flags or rates can overcharge or undercharge.
- Custom plan overrides can quietly bypass base plan limits.
- Production changes should be treated as revenue-impacting.

## Input / Output
| Input | Output |
| --- | --- |
| Stripe event, org plan, reservation usage | Updated billing state and usage credits |

## Resources
- [[Database Tables/Database Table Index]]
- [[Concepts/Billing Credits And Plans]]
- [[Workflows/Billing Usage And Session Limits]]
- [[Technologies/Stripe Billing]]

## Related Topics
- [[Database Tables/Reservations Table]]
- [[Database Tables/Organizations And Memberships]]
- [[Concepts/Webhooks]]
- [[Concepts/Production Database Safety]]

## Evidence
- supabase-kfl/supabase/migrations/20260210120000_billing.sql
- supabase-kfl/supabase/migrations/20260211120000_billing_fixes.sql
- supabase-kfl/supabase/migrations/20260225120000_plan_overrides.sql
- supabase-kfl/supabase/migrations/20260507150000_plan_overrides_credit_grant_mode.sql
- modal_kflapi/app.py
- modal_kflapi_tests/test_billing_webhooks.py
- platform/src/pages/usage.tsx
