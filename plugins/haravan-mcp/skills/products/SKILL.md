---
name: products
description: Search or retrieve Haravan products using the permission-filtered API guidance.
---

Discover the permission-filtered `llms.txt` document for product search or retrieval. Inspect the original content before supplying SKU, barcode, or product-id values, then execute with the document ID. Return only the requested product fields.

Use the documented `fields` parameter whenever available. For an uncertain
response shape, make one `limit=1` probe, select the required fields, and then
run the actual query with projection. Avoid variants, inventory, images, or
metafields unless they are needed to answer the question.
