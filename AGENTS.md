# Project Overview

This project analyzes weekly demand data for a meal delivery company to understand demand trends, fulfillment center performance, and the role of promotional offers.

## Raw Data Files

The original files are located in `data/raw`:

- `train.csv` — Historical training data containing weekly meal-order observations.
- `test.csv` — Test data containing observations for which order demand is to be predicted.
- `meal_info.csv` — Reference information describing meals.
- `fulfilment_center_info.csv` — Reference information describing fulfillment centers.

Use the files' actual contents and documented schema to confirm available fields when analysis begins; do not assume fields that have not been verified.

## Analysis Tools

Use Python and Jupyter Notebook for the analysis.

## Working Rules

- Do not modify the original data files.
- Do not fabricate data or results.
- After joining data, verify the row count and the total number of orders.
- Document data-cleaning decisions.
- Do not treat statistical correlation as evidence of causation.
- Save visualizations in `reports/figures`.
- Write clear analysis that can be rerun.
