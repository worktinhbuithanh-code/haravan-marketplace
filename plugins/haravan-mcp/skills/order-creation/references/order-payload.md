# Haravan order creation payload

Source: [Haravan Order API](https://docs.haravan.com/docs/omni-apis/orders/).

The create endpoint is:

```text
POST https://apis.haravan.com/com/orders.json
```

The minimum documented payload is an `order` object containing one or more line
items. Each simple line item uses a `variant_id` and a positive `quantity`:

```json
{
  "order": {
    "line_items": [
      {
        "variant_id": 1061514693,
        "quantity": 1
      }
    ]
  }
}
```

## Field policy

| Field | Use | Rule |
| --- | --- | --- |
| `line_items[].variant_id` | Identify the sellable variant | Resolve from documented product/variant data; do not guess from SKU or name. |
| `line_items[].quantity` | Quantity ordered | Must be positive; preserve the user's requested quantity. |
| `line_items[].product_id` | Optional product context | Include only when documented or returned by the variant lookup. |
| `line_items[].price`, `title`, `sku`, `barcode` | Custom/manual line item data | Do not override variant data unless the user explicitly requests a documented custom line item. |
| `customer.id` | Attach an existing customer | Resolve or verify the customer first; do not silently create one. |
| `email` | Order contact and optional receipt target | Include when supplied and when receipt behavior is requested. |
| `shipping_address` | Delivery destination | Use the documented address fields and location codes. Do not fabricate missing codes. |
| `shipping_lines` | Selected shipping method and price | Use a documented shipping-rate result when a rate must be calculated. |
| `discount_codes` | Apply a known discount | Include only when the user explicitly asks and the documented discount shape is available. |
| `is_cod_gateway` | COD gateway | Set `true` only for an explicitly requested COD order. |
| `gateway` | Custom/manual gateway label | Use only with an explicitly selected gateway supported by the inspected guidance. |
| `financial_status` | State recorded on the order | Never use it as proof that money was collected. |
| `fulfillment_status`, `fulfillments` | Fulfillment state/location | Include only when the user explicitly wants the order fulfilled at creation. |
| `is_confirm` | Confirm during creation | Do not set by default; use only when confirmation is explicitly requested and documented. |
| `send_receipt`, `send_fulfillment_receipt` | Customer notifications | Default to omitted/false unless the user explicitly authorizes sending them. |
| `source` / `source_name` | Order origin | Omit unless the origin is known and assignable; do not invent protected channel values. |
| `note`, `tags`, `note_attributes` | Internal context | Include only when supplied by the user. |
| `location_id` | Processing/fulfillment location | Include only when required by the selected fulfillment flow. |

## Related API calls

- Existing customer lookup: `GET /com/customers/search.json?query=...` with
  `com.read_customers`; customer creation is a separate mutation requiring
  `com.write_customers`.
- Shipping rates: `GET /com/shipping_rates.json` with the documented
  `country_id`, `province_id`, `district_id`, `total_price`, and `total_weight`
  parameters; this requires `com.read_shippings`.
- Order confirmation, when explicitly requested: `POST
  /com/orders/{order_id}/confirm.json`.

The order API requires `com.write_orders`; read access is `com.read_orders`.
Commerce write scopes include read access according to Haravan's access-scope
documentation.

Haravan documents a default rate-limit bucket of 80 requests with a leak rate
of 4 requests per second. Avoid unnecessary discovery calls and do not retry a
possibly completed POST blindly. A `429` means the request was throttled; wait
and retry only when the operation is known to be safe.
