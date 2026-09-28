---
name: customers
description: Find or retrieve Haravan customers using the permission-filtered API guidance.
---

For customer lookup, discover the relevant permission-filtered `llms.txt` document using the customer's phone, email, or name. Inspect the original document, then build the request from its instructions and execute it with that document's ID. Return only the fields needed for the user's request.

Keep customer responses projected and minimal. If the exact response shape is
unclear, probe once with `limit=1`, then repeat the real request with the
documented `fields` parameter. Do not request order history, addresses, or
other nested data unless the user explicitly asks for it.
