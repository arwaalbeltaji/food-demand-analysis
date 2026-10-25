# Food Demand Analysis

## Project Overview

This project analyzes weekly meal-order data for a meal-delivery company. It explores demand trends, the contribution of meal categories and individual meals, fulfillment-center performance, promotional patterns, price discounts, and demand variability. The analysis is descriptive and does not treat correlations as evidence of causation.

## Dataset

The analysis expects four original CSV files in `data/raw/`:

| File | Description |
|---|---|
| `train.csv` | Historical weekly meal-order observations, including `num_orders`. |
| `test.csv` | Observations for the later test period; it does not contain `num_orders`. |
| `meal_info.csv` | Reference attributes for meals, including their categories. |
| `fulfilment_center_info.csv` | Reference attributes for fulfillment centers. |

The original data files are not included in this repository. The entire `/data/` directory is excluded by `.gitignore`, including the CSV files. Obtain the original **Food Demand Forecasting** data from its original distribution, subject to its applicable access and use terms, then place the four files above in `data/raw/`. No verified download URL is documented in this project, so this README does not provide one.

## Project Structure

```text
food-demand-analysis/
├── .docs/
│   ├── analysis_findings.md
│   ├── cleaning_decisions.md
│   ├── find_skills_results.md
│   └── skill_tests.md
├── .github/
│   ├── instructions/
│   │   └── data-analysis.instructions.md
│   └── skills/
│       ├── demand-kpi-calculator/
│       │   └── SKILL.md
│       ├── find-skills/
│       │   └── SKILL.md
│       └── promotion-impact-checker/
│           └── SKILL.md
├── data/
│   └── raw/                         # Original CSVs; excluded from Git
├── notebooks/
│   └── analysis.ipynb
├── reports/
│   ├── figures/
│   │   ├── category_pareto.png
│   │   ├── center_type_performance.png
│   │   ├── promotion_demand_comparison.png
│   │   └── weekly_demand_trend.png
│   └── final_report.md
├── .gitignore
├── AGENTS.md
└── README.md
```

The local `.venv/` environment is also excluded from version control.

## Installation and Requirements

No `requirements.txt` is currently provided. Use a Python environment with Jupyter support, then install the libraries used by the notebook:

```bash
python -m pip install pandas numpy matplotlib scipy jupyter ipykernel
```

The notebook uses pandas and NumPy for data handling and calculations, SciPy for statistical tests and correlations, Matplotlib for visualizations, and IPython/Jupyter for notebook display and execution.

## How to Run

1. Open the project folder in Visual Studio Code.
2. Confirm that the four CSV files are present in `data/raw/`.
3. Install the required packages in the Python environment you intend to use.
4. Open `notebooks/analysis.ipynb` and select the matching Python kernel.
5. Run the notebook from top to bottom using **Run All**. Sections depend on DataFrames and results created by earlier cells.

The notebook locates the project data and figure directories relative to the current project folder, so it can be started from either the project root or `notebooks/`.

## Analysis Sections

The notebook contains six analysis areas:

1. **Weekly demand trends:** Weekly totals, four-week moving average, period comparisons, and Spearman correlation.
2. **Category and meal Pareto analysis:** Order totals, contribution shares, and cumulative concentration.
3. **Fulfillment-center performance:** Center-type and center-level demand, including per-center averages and operating-area association.
4. **Promotion and demand analysis:** Separate email and homepage promotion comparisons, including within-meal-and-center descriptive comparisons and exploratory Mann–Whitney U tests.
5. **Discount and demand correlation:** Row-level discount percentages, overall and category-level Spearman correlations, and descriptive discount bands.
6. **Demand volatility (CV):** Meal- and center-level weekly variability using observed weeks only.

Data-quality checks, validated many-to-one joins, KPI calculations, and figure display are included in the notebook workflow.

## Key Findings

Results below were calculated from the project data and are also summarized in the final report:

- **Total orders:** 119,557,485; **average weekly orders:** 824,534.38 across 145 training weeks.
- The five leading meal categories contributed **81.40%** of orders. The top 22 of 51 meals contributed **81.18%**.
- **TYPE_B** had the highest average total orders per center, at 1,999,738.20. TYPE_A had the largest aggregate total, reflecting its larger center count.
- Within meal-center pairs observed both with and without a promotion, the median difference in mean orders was +142.14% for email promotion and +122.31% for homepage featuring. These are observational comparisons, not established causal effects.
- Overall discount percentage had a weak positive Spearman association with orders (`rho = 0.1218`). Associations varied by category and do not establish causality.

See [the final report](reports/final_report.md) for detailed methods, results, visualizations, recommendations, and limitations.

## Results and Visualizations

The final Arabic-language report is available at [reports/final_report.md](reports/final_report.md).

| Figure | Description |
|---|---|
| [Weekly demand trend](reports/figures/weekly_demand_trend.png) | Weekly orders and four-week moving average |
| [Category Pareto chart](reports/figures/category_pareto.png) | Category order totals and cumulative contribution |
| [Center type performance](reports/figures/center_type_performance.png) | Average total orders per center by type |
| [Promotion demand comparison](reports/figures/promotion_demand_comparison.png) | Median within-pair percentage differences for promotion observations |

## Optional Bonus: Demand Forecasting

A Random Forest model was trained on weeks 1–135 and temporally validated on weeks 136–145. On the validation period, the Naive Baseline achieved an MAE of **110.3495**, while Random Forest achieved an MAE of **69.4827**, an improvement of **37.0339%** over the baseline.

The final Random Forest model was retrained on weeks 1–145 and used to generate predictions for **32,573** test rows covering weeks 146–155. The predictions are saved in [reports/predictions.csv](reports/predictions.csv) with columns `id` and `num_orders`.

These predictions can support weekly inventory and procurement planning. Results should be interpreted with care: evaluation used only one temporal validation period, future accuracy is not guaranteed, and Random Forest has limited ability to extrapolate beyond the training period.

## AI-Assisted Workflow

GitHub Copilot assisted with preparing analysis code, organizing notebook sections, reviewing calculations, and drafting project documentation. Reported metrics and findings were checked against the actual project data; Copilot assistance is not a substitute for reviewing the data, assumptions, or conclusions.

Project-specific guidance includes:

- `AGENTS.md` for project purpose and data-analysis working rules.
- `.github/instructions/data-analysis.instructions.md` for source preservation, join validation, causal interpretation, and reproducibility.
- `.github/skills/demand-kpi-calculator/` and `.github/skills/promotion-impact-checker/` for project-specific KPI and promotion-analysis workflows.
- `.github/skills/find-skills/` for skill discovery guidance.

## Limitations

- Week values are sequence numbers, not identified calendar dates.
- City and region values are codes; geographic names are not provided.
- Correlations and promotion comparisons are observational and do not establish causation. Repeated weekly observations may be dependent, and other factors may confound comparisons.
- Some meals and centers are not observed in every week. Such absent weeks were excluded from CV calculations rather than assumed to represent zero orders.
- The currency for the recorded prices is unspecified; calculated revenue is an estimate, not confirmed accounting revenue.
- The average discount is an arithmetic mean over rows, not weighted by order volume. Negative discount percentages were retained.
