# Order status

Each order has one of three statuses:

| Status | Meaning |
| --- | --- |
| `completed` | Delivered |
| `shipped` | Sent, not yet delivered |
| `cancelled` | Cancelled; no money was collected |

The metrics count every order, whatever its status. A cancelled order still adds to `order_count` and `revenue`. To leave cancelled orders out, filter on `orders.status`.

Choose the filter from the question's wording:

- **"Sales" or "revenue", with no qualifier:** exclude cancelled orders, `orders.status != 'cancelled'`, and say that you did.
- **"Bookings" or "all orders placed":** include every status.
- **"Delivered" or "fulfilled":** `completed` only.
