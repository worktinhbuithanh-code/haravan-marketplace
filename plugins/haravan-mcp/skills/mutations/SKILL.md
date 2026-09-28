---
name: mutations
description: Create, update, or delete Haravan resources when the user explicitly requests a documented write operation.
---

Use this skill only for an explicit Haravan create, update, or delete request.
For each mutation, discover one focused resource/workflow intent, inspect the
original document, retain its `document_id` and `version`, then call
`haravan_execute_api` with the documented request and a stable
`idempotency_key`. Do not assume that read-oriented filters, pagination, or
projection apply to a mutation.

Before executing, establish the exact resource, record or records, fields to
change, and intended scope from the user's request and the inspected guidance.
If any of these are ambiguous, ask a focused clarification question. Do not
invent an endpoint, method, request body, or status value. Generate one opaque,
stable `idempotency_key` for the intended mutation and reuse it only when
retrying that same logical operation.

`haravan_execute_api` prepares every mutation and returns a confirmation plan;
it does not send the request. Show that plan to the user and call
`haravan_confirm_mutation` with its exact `confirmation_id` only after explicit
confirmation. This confirmation step applies to create, update, delete, and
bulk mutations. For a create or update, prepare only when the requested change
is unambiguous. After confirmation, report the returned identifier and status,
and distinguish a server error or partial result from success.
