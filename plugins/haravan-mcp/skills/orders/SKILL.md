---
name: orders
description: Search, retrieve, and summarize Haravan orders using documented, permission-filtered API guidance.
---

Use this skill for order retrieval, order lists, and order-level summaries. For
explicit order creation, use `order-creation` instead. For each
API-backed request, discover the relevant permission-filtered guidance,
inspect the original document, then execute only the documented request with
its `document_id` and inspected `version`. Keep each discovery query focused on one order resource or
workflow stage; do not submit a compound business sentence as one discovery
intent. Keep the instructions below for order-specific choices.

Use documented date, payment, fulfillment, and status filters. Never invent a
filter, endpoint, status value, or response field. Choose the smallest fields
that answer the question:

- order count: `id`
- revenue or business summary: order date, amount, currency, and relevant
  payment/fulfillment statuses
- best-selling products: only the documented line-item fields needed for
  product and quantity aggregation

For aggregates, retrieve every required page with the same filters and fields
before counting, grouping, or summing. Keep date boundaries explicit, including
the timezone when it affects the result. Return a concise summary rather than
reproducing raw order JSON.
