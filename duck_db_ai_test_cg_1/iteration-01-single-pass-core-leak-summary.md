# Known quirks

These dataset facts matter in the light build.

## Verified facts

- sessions: 472,871
- pageviews: 1,188,124
- orders: 32,313
- refunded order items: 1,731

## Quirks to honor

1. Null `utm_source` and `utm_campaign` values exist.
   - Normalize them to `direct_or_unknown`.
   - Do not filter them out.

2. Entry page must be derived from the earliest pageview in each session.
   - Order by `created_at`, then `website_pageview_id`.

3. Refunds arrive after orders.
   - Use the actual refund table without clipping it to the session window.

4. The light build intentionally ignores `order_items` and `products`.
   - They remain available in the bundle if scope later expands.

5. Keep the final aggregation at month grain.
   - Month grain is more stable and easier for mid-range agents to generate correctly than day grain.
