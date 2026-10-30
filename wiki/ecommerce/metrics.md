# Metrics

Each metric lists four properties:

- **Grain:** the row it counts or adds up.
- **Time basis:** the field to use when slicing it by time.
- **Aggregation:** how it combines across groups, and whether the group results can be added together.
- **Constraints:** what it includes, and how not to use it.

## `order_count`

The number of orders: `count(orders.id)`.

- **Grain:** one order.
- **Time basis:** `orders.ordered_at`, the local order date.
- **Aggregation:** count. Results add up across customer, market, status and date groups. They do not add up across product groups: an order with several products is counted once in each product's group.
- **Constraints:** includes every status, cancelled orders too. See [Order status](order-status.md).

## `units_sold`

The number of units sold: `sum(order_items.quantity)`.

- **Grain:** one order line.
- **Time basis:** `orders.ordered_at`.
- **Aggregation:** sum. Results add up across every group.
- **Constraints:** includes every status.

## `revenue`

Gross revenue: the sum of quantity × list price over order lines.

- **Grain:** one order line.
- **Time basis:** `orders.ordered_at`.
- **Aggregation:** sum. Results add up across every group.
- **Constraints:**
  - Gross: before the order discount. Use `net_revenue` for what customers were charged.
  - Includes every status.
  - The price is the product's current list price, `products.price`. Order lines store no price of their own.

## `net_revenue`

Revenue after the order discount: the sum of quantity × list price × (1 − `discount_pct` / 100) over order lines.

- **Grain:** one order line.
- **Time basis:** `orders.ordered_at`.
- **Aggregation:** sum. Results add up across every group.
- **Constraints:** includes every status. The discount is set per order and applies to each of its lines.

## `aov`

Average order value: `revenue` ÷ `order_count`.

- **Grain:** one order.
- **Time basis:** `orders.ordered_at`.
- **Aggregation:** ratio. Results do not add up, and averaging them is wrong too. For a larger group, compute `aov` again for that group.
- **Constraints:**
  - Gross, and includes every status.
  - Grouped by product, it is that product's revenue ÷ the orders that contain it. That is not the value of whole orders.

## `monthly_target`

The revenue target for one calendar month: the sum of `markets.monthly_target` over the markets in scope.

- **Grain:** one market, for one month.
- **Time basis:** none. A market's target is the same every month and has no date.
- **Aggregation:** sum across markets. It does not add up over time: grouped by month, each month shows the same full target.
- **Constraints:**
  - Compare it with one month of revenue in the same markets. For several months, multiply it by the number of months.
  - Slice it only by `markets` fields, such as market and region.
  - The data does not say whether targets are gross or net. Compare them with `revenue`, and say so.

## `session_count`

The number of website sessions: `count(sessions.id)`.

- **Grain:** one session.
- **Time basis:** `sessions.started_at`, in UTC.
- **Aggregation:** count. Results add up across session and market groups.
- **Constraints:**
  - Includes every funnel step.
  - Sessions relate only to `markets`. Slicing by customer, order or product fields gives wrong results.

## The `order_value` dimension

`orders.order_value` is one order's gross revenue. It is a field of the order, not a metric: use it to group orders into bands or to draw a histogram.
