---
name: general
description: Use Haravan MCP when a request requires discovering permission-filtered API guidance and executing a documented request; route entity-specific work to a specialized skill.
---

Apply this skill only when the answer requires Haravan data or a Haravan action. Do not call the API for general explanations, calculations over data already provided by the user, or connection-status questions that can be answered by the server's status tool.

## Common API protocol

For an API-backed request, first split the work into atomic API intents. A
compound task such as creating an order may contain separate stages for
variant resolution, customer lookup, shipping-rate lookup, and order
creation.

1. Call `haravan_discover_api` with one focused `query` for a single intent.
   For a compound task, use the tool's `queries` form with one focused query per
   resource or workflow stage. Discovery returns documents only; it does not
   authorize execution or replace inspection.
2. Inspect each returned document with `haravan_inspect_api` and retain both
   its `document_id` and `version`.
3. Build each request only from the original inspected `llms.txt` content.
4. Call `haravan_execute_api` with that document's `document_id`, method, URL,
   query, body, and inspected `version`. For a mutation, also provide one
   stable `idempotency_key` for that logical operation.

For read requests, decide the smallest output contract first. Use documented
filters and `fields` when the endpoint supports them. Do not fetch nested
objects unless they are needed for the answer. If the response shape is
uncertain, make one `limit=1` probe only when the documented endpoint supports
that kind of probe; otherwise follow the documented response shape directly.
Keep filters and projection consistent while paginating, and aggregate locally
only after all required pages are available. Do not reuse a document discovered
for one resource or stage to execute a different resource or stage.

For write requests, use only a documented mutation. `haravan_execute_api` does
not send a mutation: it returns a confirmation plan and `confirmation_id`.
Show the exact scope and payload summary to the user, and call
`haravan_confirm_mutation` only after the user explicitly confirms that plan.
Reuse the same `confirmation_id` only for that plan; do not reconstruct or
re-execute the mutation. Report the result using the returned identifiers or
status; do not claim success from an unverified request.

Never infer a path, method, scope, or shop identity from conversation text. Do
not send `Authorization`, access tokens, credential references, or a shop
selector. The backend reloads the document, checks its roles, binds the request
to the authenticated installation, and injects Haravan authentication
server-side.
