# Maintaining the wiki

These files hold the business rules behind the `ecommerce` dataset. Keep them consistent with the dataset:

- When you add or change a metric in `datasets/ecommerce/ecommerce.dataset.aml`, update its entry in [metrics.md](metrics.md) with its grain, time basis, aggregation and constraints. You are done when every metric in the dataset has all four.
- Keep the wiki to rules. Numbers from the data, such as row counts, date ranges or value shares, belong in queries, where they stay current.
- Give every model, field and metric a `label` and a `description` in the AML, so the wiki and the dataset describe the same names.
- Add a new topic as its own file, and link it from [README.md](README.md).
