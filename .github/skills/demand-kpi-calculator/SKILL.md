---
name: demand-kpi-calculator
description: Calculate and explain core meal-demand performance indicators from the project's verified data, including order volume, weekly demand, revenue, discounts, and promotional coverage. Use only when asked to calculate these KPIs from actual project data.
---

# Demand KPI Calculator

## Purpose and Scope

Use this skill to calculate and explain core performance indicators for meal demand using the project's actual data. It covers data verification, KPI calculations, result validation, and clear presentation. Do not use it to discover or install agent skills, perform unrelated exploratory analysis, or modify source data.

## Workflow

1. **Inspect the available data and schema before calculating.**
   - Identify the relevant project data files and read their structure and column names.
   - Confirm which file or combined dataset contains the fields required for each KPI.
   - Check data types, row counts, missing values, and whether joins are required.
   - Do not assume column names or schemas. If required fields are absent or ambiguous, explain what is missing and ask for clarification rather than guessing.

2. **Preserve source data.**
   - Treat original CSV files as read-only. Never overwrite or modify them.
   - Perform any necessary calculations on in-memory data or a separate derived object.
   - Do not fabricate, infer, or fill in data or results without an explicitly justified and documented method.

3. **Calculate the requested KPIs using verified fields.**
   - **Total orders:** Sum `num_orders` across the records in the defined analysis scope. State the scope and any exclusions.
   - **Average weekly orders:** Aggregate `num_orders` by the verified week field, then calculate the arithmetic mean across the included weeks. Report how many weeks were included and how missing weeks, if any, were handled.
   - **Revenue:** Sum `checkout_price × num_orders` across included records. Treat this as revenue calculated from the provided checkout prices and order counts; do not imply it is accounting-recognized revenue.
   - **Average discount percentage:** For records with a valid, positive `base_price`, calculate `((base_price - checkout_price) / base_price) × 100`, then report the arithmetic mean of those valid record-level percentages. Exclude zero, negative, missing, or non-numeric `base_price` values from this KPI and state how many records were excluded. Do not silently clamp negative discounts or otherwise alter valid values.
   - **Promotional-record percentage:** Identify the project's verified promotion/offer field and define which values count as promotional before calculating. Divide the number of promotional records by the number of records in the defined scope with a usable value for that field, then multiply by 100. State the denominator and how missing or ambiguous values were treated. If no suitable promotion field is available, report that the KPI cannot be calculated from the available data.

4. **Validate the calculations.**
   - Check that inputs used in each KPI are available and that calculated values are finite and consistent with the selected scope.
   - Reconcile total orders to the sum of the included `num_orders` values.
   - Confirm the number of weekly aggregates used for the weekly average.
   - Confirm revenue is computed from row-level price-times-orders products before summation.
   - For percentage KPIs, verify the denominator and excluded-record counts; percentages should be within 0–100 only when the metric's definition and data support that range. Explain any result outside it rather than changing it.
   - If joining files, validate join keys and report row counts and total orders before and after the join so unintended duplication or row loss is visible.
   - Never invent missing values, assumptions, or KPI results. Surface calculation limitations explicitly.

## Presenting and Interpreting Results

Present each KPI with its name, value, unit, analysis scope, and relevant denominator or record count. Use clear rounding for display while retaining full precision for calculations. Briefly explain what each KPI measures:

- Total orders measures the summed order volume in scope.
- Average weekly orders describes the mean order volume per included week, not per record.
- Calculated revenue estimates the sum of checkout price multiplied by orders.
- Average discount percentage summarizes valid record-level discounts relative to base price.
- Promotional-record percentage is the share of eligible records classified as promotional.

Distinguish calculations from interpretation. Do not claim that promotions caused higher demand or revenue based only on these descriptive KPIs; statistical association alone does not establish causation.
