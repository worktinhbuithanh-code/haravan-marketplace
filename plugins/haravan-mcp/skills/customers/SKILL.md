---
name: customers
description: Search or retrieve Haravan customers using documented, permission-filtered API guidance.
---

Use this skill for customer search and retrieval. For each API-backed request,
discover one focused customer intent, inspect the original permission-filtered
document, then execute only the documented request with its `document_id` and
inspected `version`. Use only lookup criteria and response fields documented by
that guidance.

Keep customer responses projected and minimal. Probe once only if the
documented search endpoint supports it and the response shape is genuinely
unclear. Do not request order history, addresses, or other nested data unless
the user explicitly asks for it and the inspected guidance supports it.
