---
name: products
description: Search or retrieve Haravan products using documented, permission-filtered API guidance.
---

Use this skill for product search and retrieval. For each API-backed request,
discover one focused product or variant intent, inspect the original
permission-filtered document, then execute only the documented request with
its `document_id` and inspected `version`. Inspect the guidance before
supplying SKU, barcode, product-id, or other product identifiers.

Use the documented `fields` parameter whenever available. Probe once only when
the documented endpoint supports it and the response shape is uncertain. Avoid
variants, inventory, images, or metafields unless they are needed to answer the
question and are documented for that request.
