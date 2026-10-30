# Dates and time zones

| Field | Type | Meaning |
| --- | --- | --- |
| `orders.ordered_at` | date | The order date in the customer's local time. Use it for days, weeks, months and fiscal periods. |
| `orders.ordered_ts` | datetime | The order instant in UTC. Use it for hour of day, or to compare orders across time zones. |
| `sessions.started_at` | datetime | The session start in UTC. |
| `customers.created_at` | date | The customer's sign-up date. |

An order's UTC date can differ from its local date. Group by `ordered_at`, not by the date of `ordered_ts`, unless the question asks for UTC.

Before comparing months, check the latest order date: the last month in the data may be partial. Leave a partial month out of comparisons, or say that it is partial.
