# Funnel

`sessions.reached_step` is the furthest step a website visit reached:

| Step | Meaning |
| ---: | --- |
| 1 | Visited |
| 2 | Viewed a product |
| 3 | Added to cart |
| 4 | Started checkout |
| 5 | Ordered |

- The sessions that **reached** a step are those with `reached_step >= step`, not only those that stopped there.
- Every order has exactly one session at step 5. So the conversion rate is the sessions at step 5 ÷ all sessions.
- `sessions.channel` is where the visit came from: `Organic`, `Paid search`, `Email` or `Social`.
