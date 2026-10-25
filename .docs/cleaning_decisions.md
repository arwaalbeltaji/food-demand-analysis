# Data Quality Findings and Cleaning Decisions

## Scope

This document records the initial data-quality checks and preliminary cleaning decisions for the food-demand-analysis project. The decisions are based only on the verified findings listed below. They describe data handling and do not constitute demand or promotional-effect analysis.

## Verified Data Quality Findings

- `train.csv` contains 456,548 rows and its `num_orders` sum is 119,557,485.
- `test.csv` contains 32,573 rows.
- No missing values were found in any of the four source files.
- No fully duplicated rows were found. No duplicate `(week, center_id, meal_id)` combinations were found in `train.csv` or `test.csv`.
- No zero or negative prices were found.
- `checkout_price` is greater than `base_price` in 116,101 training rows and 8,937 test rows.
- `emailer_for_promotion` and `homepage_featured` contain only 0 and 1.
- In `train.csv`, `num_orders` ranges from 13 to 24,299; the median is 136, Q1 is 54, and Q3 is 324. The upper IQR fence is 729, with 32,937 rows above it.
- Training weeks range from 1 to 145, and test weeks range from 146 to 155. There are no missing week numbers within either file's range.
- The many-to-one joins succeeded without changing row counts or the training `num_orders` sum, and there were no unmatched keys.

## Cleaning Decisions

1. **Do not delete or impute values at this stage.** No missing values or confirmed duplicate records were found, so there is no current evidence justifying deletion or imputation.
2. **Retain records where `checkout_price > base_price`.** These observations are not automatically data errors; they may represent valid pricing situations. Revisit only if additional evidence establishes a data problem.
3. **Retain high-order observations identified by the IQR rule.** Exceeding the statistical upper fence does not by itself prove an error. Do not remove or alter these records solely because they exceed 729 orders.
4. **Do not add synthetic weeks or rows.** The observed week sequences are complete within each file's range; no artificial observations will be introduced.
5. **Keep original CSV files unchanged.** Any future cleaning or transformation must operate in memory or on a derived copy, not overwrite files in `data/raw`.
6. **Interpret discount calculations carefully.** When calculating discount percentages later, distinguish positive discounts from cases where the checkout price exceeds the base price. Do not convert negative percentages to zero without documenting and justifying that transformation.
7. **Treat these decisions as preliminary.** They are based on initial quality checks and may be revised if new evidence emerges during analysis. Any revision must be explained and documented.

## Validation and Integrity Checks

- The source row counts and training order total were checked against the verified values above.
- Checks for missing values, fully duplicated rows, duplicate week-center-meal combinations, non-positive prices, and promotion-flag values were completed.
- Week ranges and missing week numbers within each file's observed range were checked.
- Join validation confirmed many-to-one relationships, no unmatched keys, unchanged row counts, and an unchanged training order total.
- No original source files were modified during these checks.

## Limitations

- These checks establish only the conditions explicitly examined; they do not prove that every value is semantically correct or that the data is free of all quality issues.
- Values above the IQR fence and checkout prices above base prices are flagged for awareness, not classified as errors.
- No causal conclusions about promotions or demand can be drawn from these data-quality checks.
- Cleaning choices should be reconsidered if later analysis, domain evidence, or additional validation reveals a specific issue.
