# Agent instructions

This repo is an anfra sample: a made-up online store, modelled in AML as one dataset, `ecommerce`, over a read-only DuckDB file. The data is synthetic, so present every finding as sample data.

## Answering a question

Follow these steps for every question about the data:

1. Read the files in [wiki/ecommerce/](wiki/ecommerce/README.md) that the question touches. You are done when you know which metric, status filter and date field the question needs.
2. Run `anfra show ecommerce` to get the exact field and metric names.
3. Profile the data for every number your answer relies on, such as row counts, date ranges and value shares. The wiki holds rules, not results.
4. Check the query with `anfra query validate`, then run it with `anfra query`.
5. In your answer, name each wiki default you applied.

## Running anfra

These are the gotchas that `--help` does not tell you:

- Run anfra from the repo root. It reads `.anfra/` from the current directory.
- After you change a file under `models/` or `datasets/`, run `anfra validate` until it reports `valid: true`.
- When an error names a model, field or dataset that disagrees with the files on disk, the compile cache is stale, usually after a move or rename. Clear it with `rm -rf ~/.anfra/repos/<folder>-*`, where `<folder>` is this repo's folder name, then run the command again.

## Changing the sample

Keep the sample consistent with its wiki:

- Treat `data/ecommerce.duckdb` as fixed input. anfra opens it read-only.
- Give every model, field and metric a `label` and a `description`. Agents choose fields by them.
- When you add or change a metric, update its entry in [wiki/ecommerce/metrics.md](wiki/ecommerce/metrics.md) with its grain, time basis, aggregation and constraints. You are done when every metric in the dataset has all four.
- Keep the wiki to rules. Numbers from the data belong in queries, where they stay current.

## Building a Data App

The repo has no Data Apps. To build one on the `ecommerce` dataset, run `anfra skills install` first. The skills cover writing and previewing apps.
