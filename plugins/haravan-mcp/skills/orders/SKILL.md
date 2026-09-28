---
name: orders
description: Answer Haravan order questions using the permission-filtered API guidance.
---

For order questions, discover and inspect the relevant permission-filtered `llms.txt` document. Build the order request from its original content, including status or fulfillment filters when documented, and execute it with the document ID. Do not invent undocumented filters or endpoints.

## Response-size discipline

Never request or pass through the full raw order JSON by default. First map the
user's request to the smallest fields that prove the answer, then send them in
the documented `fields` query parameter.

Use this decision process:

1. If the document already shows the needed response shape, skip probing and
   request the exact fields immediately.
2. If a field name, nesting, or value shape is uncertain, run one probe with
   the same date/status filters and `limit=1` (and `page=1`). Use the probe to
   confirm the shape, then discard it and run the real request with `fields`.
3. For a list requested by the user, use the requested limit. For statistics,
   use `limit=50`, keep the same `fields` on every page, and paginate until the
   result is complete before counting, grouping, or summing.
4. Do not request `customer`, addresses, `transactions`, `fulfillments`, or
   `line_items` for a KPI that does not use them.

For “tình hình kinh doanh 7 ngày qua” or a similar order summary, interpret
the date range explicitly and normally request only:

```text
fields=id,created_at,total_price,currency,financial_status,fulfillment_status
limit=50&page=1&created_at_min=<START>&created_at_max=<END>
```

Then calculate order count, revenue totals by currency, and status counts from
the projected results. If the user asks for best-selling products, add only the
documented `line_items` fields needed for quantity/product aggregation. If the
user asks only for the number of orders, request `fields=id`.

Use the same time boundaries, filters, and projection while incrementing
`page`; do not re-fetch each page as raw JSON. Return a concise summary rather
than reproducing API payloads.
