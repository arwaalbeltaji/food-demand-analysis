---
applyTo: "**/*.py,**/*.ipynb"
---

# Data Analysis Rules

## Rule 1 — Preserve Raw Data

- Do not modify or replace original CSV files in `data/raw`.
- Perform data cleaning and transformations only on derived copies or in memory.

## Rule 2 — Validate Joins

- After every data join, verify and record the row count and the sum of `num_orders` before and after the join.
- Do not continue the analysis if data is unexpectedly lost or duplicated. First identify and address the cause, and document the issue and its resolution.

## Rule 3 — Avoid Causal Claims

- Do not interpret correlations or statistical comparison results as proof of causation.
- State potential limitations when interpreting promotional results, including confounding factors and limitations of the comparison or study design.

## Rule 4 — Reproducibility

- Use only the project's actual data; do not invent or substitute data or results.
- Make code reproducible and document assumptions and data-cleaning decisions.
