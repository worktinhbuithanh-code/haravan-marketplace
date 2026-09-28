---
name: mutations
description: Create, update, or delete Haravan resources when the user explicitly requests a documented write operation.
---

Use this skill only for an explicit Haravan create, update, or delete request.
For each mutation, discover the relevant permission-filtered guidance, inspect
the original document, then execute only the documented request with its
`document_id`. Do not assume that read-oriented filters, pagination, or
projection apply to a mutation.

Before executing, establish the exact resource, record or records, fields to
change, and intended scope from the user's request and the inspected guidance.
If any of these are ambiguous, ask a focused clarification question. Do not
invent an endpoint, method, request body, status value, or idempotency key.

Require explicit confirmation immediately before a destructive operation or a
broad bulk change. For a create or update, execute only when the requested
change is unambiguous. After execution, report the returned identifier and
status, and distinguish a server error or partial result from success.
