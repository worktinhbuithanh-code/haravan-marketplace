---
name: general
description: Use the Haravan MCP tools to discover permission-filtered API guidance, inspect the source document, and execute a documented request.
---

For a Haravan request, call `haravan_discover_api` with a concise description of the goal, then call `haravan_inspect_api` for the returned document. Build the HTTP intent from the original `llms.txt` content and call `haravan_execute_api` with that document's `document_id`, method, URL, query, and body.

Optimize every read request for response size:

1. Decide the smallest output contract before calling the API: exact metrics,
   identifiers, statuses, dates, or columns needed to answer the user.
2. Use documented server-side filters and the `fields` query parameter to
   request only those fields. Do not fetch customers, addresses, transactions,
   line items, or other nested objects unless the answer needs them.
3. If the response shape or a field's actual name is unclear, make one cheap
   probe with the same filters and `limit=1`. Inspect that one result, choose
   the needed fields, then make the real paginated request with `fields`.
   Never use a full unprojected response as the normal workflow.
4. Keep the selected `fields` identical across pages. Aggregate locally only
   after all required pages have been retrieved, and retain only the values
   needed for the final answer.

For example, a seven-day business summary normally needs order identifiers,
date, amount, currency, and payment/fulfillment status—not customer profiles,
shipping addresses, or raw order objects. A suitable order query is usually
`limit=50&page=1&created_at_min=<START>&created_at_max=<END>&fields=id,created_at,total_price,currency,financial_status,fulfillment_status`.
Adjust the field set to the user's requested metric.

Never infer a path, method, scope, or shop identity from conversation text. Do not send `Authorization`, access tokens, credential references, or a shop selector. The backend reloads the document, checks its roles, binds the request to the authenticated installation, and injects Haravan authentication server-side.
