# Anfra sample: e-commerce

A small online shop, ready to query with [anfra](https://github.com/holistics/anfra). `anfra init --with-sample` downloads this repo. You can also clone it and use it as it is:

```sh
git clone https://github.com/holistics/anfra-sample-ecommerce.git
cd anfra-sample-ecommerce
anfra validate
```

Run anfra from the repo root. It reads `.anfra/` from the current directory.

## What it contains

| Path | What it is |
| --- | --- |
| `data/ecommerce.duckdb` | The data: a DuckDB file, opened read-only. No database server or credentials needed. |
| `.anfra/data_sources.yml` | One data source, `ecommerce_duckdb`, pointing at the DuckDB file. It has no credentials, so it is committed. |
| `.anfra/context_sources.yml` | What `anfra ingest` reads to build the search catalog. |
| `models/ecommerce/` | Six AML models, one per table. |
| `datasets/ecommerce/ecommerce.dataset.aml` | The `ecommerce` dataset: the models, how they join, and the shared metrics. |
| `wiki/ecommerce/` | The business rules behind the metrics and fields, one Markdown file per topic. Read them before writing queries. |
| `AGENTS.md` | Instructions for coding agents working in this repo. |

### Models

| Model | Rows | One row per |
| --- | ---: | --- |
| `customers` | 200 | customer, with country and sign-up date |
| `orders` | 1,500 | order, with status, order date (June 2025 to September 2026), discount and fiscal year/quarter |
| `order_items` | 3,020 | product line on an order, with quantity |
| `products` | 8 | product, with category, subcategory, brand and price |
| `markets` | 6 | country customers come from, with region, UTC offset and a monthly revenue target |
| `sessions` | 13,500 | website visit, with channel and the furthest funnel step reached (1 visit to 5 ordered) |

Joins: `orders → customers`, `order_items → orders`, `order_items → products`, `customers → markets` and `sessions → markets`.

### Metrics

`order_count`, `units_sold`, `revenue` (quantity × price), `net_revenue` (after the order discount), `aov` (revenue ÷ order count), `monthly_target` and `session_count`. The dataset also has an `order_value` dimension: one order's revenue, for bands and histograms.

## Example queries

See the dataset's models, fields and metrics:

```sh
anfra show ecommerce
```

Revenue by product category:

```sh
anfra query -d ecommerce 'products | group(products.category) | select(products.category, revenue)'
```

| category | revenue |
| --- | ---: |
| Services | 578,000.00 |
| Software | 497,394.00 |
| Hardware | 193,999.60 |
| Parts | 13,659.35 |

Orders by status:

```sh
anfra query -d ecommerce 'orders | group(orders.status) | select(orders.status, order_count)'
```

Revenue and average order value by region:

```sh
anfra query -d ecommerce 'markets | group(markets.region) | select(markets.region, revenue, aov)'
```

## Next steps

- Add your own Data App under `apps/`, or ask a coding agent to build one (`anfra skills install` adds the anfra skills to your agents).
- Point `.anfra/data_sources.yml` at your own warehouse and write models for it next to these.
