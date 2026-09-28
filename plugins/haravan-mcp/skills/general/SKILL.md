---
name: general
description: Use the Haravan MCP tools to discover permission-filtered API guidance, inspect the source document, and execute a documented request.
---

For a Haravan request, call `haravan_discover_api` with a concise description of the goal, then call `haravan_inspect_api` for the returned document. Build the HTTP intent from the original `llms.txt` content and call `haravan_execute_api` with that document's `document_id`, method, URL, query, and body.

Never infer a path, method, scope, or shop identity from conversation text. Do not send `Authorization`, access tokens, credential references, or a shop selector. The backend reloads the document, checks its roles, binds the request to the authenticated installation, and injects Haravan authentication server-side.
