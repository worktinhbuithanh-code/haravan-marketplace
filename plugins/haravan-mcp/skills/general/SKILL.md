---
name: general
description: Use Haravan MCP when a request requires discovering permission-filtered API guidance and executing a documented request; route entity-specific work to a specialized skill.
---

Apply this skill only when the answer requires Haravan data or a Haravan action. Do not call the API for general explanations, calculations over data already provided by the user, or connection-status questions that can be answered by the server's status tool.

## Common API protocol

For an API-backed request:

1. Call `haravan_discover_api` with a concise description of the goal.
2. Call `haravan_inspect_api` for the returned document.
3. Build the request only from the original `llms.txt` content.
4. Call `haravan_execute_api` with that document's `document_id`, method, URL,
   query, and body.

For read requests, decide the smallest output contract first. Use documented
filters and `fields` when the endpoint supports them. Do not fetch nested
objects unless they are needed for the answer. If the response shape is
uncertain, make one `limit=1` probe only when the documented endpoint supports
that kind of probe; otherwise follow the documented response shape directly.
Keep filters and projection consistent while paginating, and aggregate locally
only after all required pages are available.

For write requests, use only a documented mutation. Confirm the exact resource,
records, fields, and intended scope before executing when any of them is
ambiguous. Require an explicit confirmation immediately before destructive or
broad bulk mutations. Report the result using the returned identifiers or
status; do not claim success from an unverified request.

Never infer a path, method, scope, or shop identity from conversation text. Do
not send `Authorization`, access tokens, credential references, or a shop
selector. The backend reloads the document, checks its roles, binds the request
to the authenticated installation, and injects Haravan authentication
server-side.
