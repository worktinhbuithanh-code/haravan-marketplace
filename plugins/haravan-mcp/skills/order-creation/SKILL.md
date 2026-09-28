---
name: order-creation
description: Create a Haravan order from explicit product, customer, shipping, payment, and confirmation details using documented permission-filtered API guidance. Use for an explicit request to create an order, not for checkout payment, order lookup, update, cancellation, or refund.
---

Use this skill only when the user explicitly wants a new Haravan order created.
For the payload shape and field rules, read
[references/order-payload.md](references/order-payload.md) before building the
request.

## Important distinction

An API-created order does not collect payment or perform a payment transaction.
If the user needs Haravan to collect payment through a real customer checkout,
use the storefront/cart/checkout flow instead of `POST /com/orders.json`. Never
claim that an API-created order is paid merely because its `financial_status`
is set to `paid`.

## Workflow

1. Define the order intent: create a back-office/API order, or complete a
   customer checkout. Stop and explain the checkout boundary when payment must
   actually be collected.
2. Gather and validate the minimum input:
   - at least one line item with a documented `variant_id` and a positive
     `quantity`;
   - an existing `customer.id` only when the order must attach to a known
     customer;
   - a shipping address when the order requires delivery, using documented
     country/province/district codes rather than guessed names or IDs.
3. If the user provides only SKU, barcode, or product name, discover and
   inspect one product/variant intent first. Resolve exactly one variant per
   line item; if a value is ambiguous, ask instead of choosing.
4. If shipping cost or method is required, discover and inspect a separate
   shipping-rate intent. Use the documented request with the destination IDs,
   order total, and total weight, then put only the selected documented rate
   into `shipping_lines`.
5. Decide optional side effects explicitly. Do not enable receipts, fulfillment,
   confirmation, discounts, custom pricing, gateway, or `is_cod_gateway` unless
   the user asked for that behavior and the inspected guidance supports it.
6. Call `haravan_discover_api` with one focused order-creation query, then call
   `haravan_inspect_api` for the returned document. Retain its `document_id`
   and `version`. Build the request only from the original `llms.txt` content.
7. Generate one stable `idempotency_key` for this intended order and call
   `haravan_execute_api` with the documented POST, `document_id`, `version`,
   and key. This prepares a confirmation plan; it does not create the order.
8. Show the returned plan and wait for explicit confirmation of that exact
   order. Then call `haravan_confirm_mutation` with the exact
   `confirmation_id`. Do not call `haravan_execute_api` again for confirmation.
9. Verify the confirmed response before reporting success. Return the API order `id`,
   customer-facing `name` or `order_number` when present, total, currency, and
   the returned payment/fulfillment/confirmation statuses.

## Safety and failure handling

- Never send authorization headers, tokens, credential references, or a shop
  selector. The backend supplies authentication and binds the request to the
  authenticated installation.
- Do not create a new customer automatically just because no customer ID was
  supplied. Create or update a customer only when that is separately requested.
- Do not mark an order paid, fulfilled, or confirmed by inference. Use the
  documented fields only when the user's intent and the source state justify
  them.
- Do not blindly retry a create request after a timeout; first determine whether
  the order was created to avoid duplicates.
- On HTTP 403, report the missing permission rather than retrying. On HTTP 429,
  respect the rate-limit response and retry only when safe. Surface 4xx/5xx
  errors with the API's actual message.
