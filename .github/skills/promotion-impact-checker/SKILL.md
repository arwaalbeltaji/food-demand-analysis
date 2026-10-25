---
name: promotion-impact-checker
description: Assess the association between email and homepage promotions and meal demand using the project's verified data, matched meal and fulfillment-center comparisons, and appropriate statistical tests. Use only when asked to evaluate promotional demand patterns.
---

# Promotion Impact Checker

## Purpose and Scope

Use this skill to examine whether weekly meal orders differ between weeks with and without email promotions and homepage promotions. Treat the results as observational associations, not causal effects. Do not use this skill to modify source data, invent findings, or perform unrelated analysis.

## Workflow

1. **Verify the data before analysis.**
   - Identify the relevant project data files and inspect their column names, data types, row counts, and missing values.
   - Confirm the fields representing week, meal, fulfillment center, order count, email promotion, and homepage promotion. Do not assume column names or meanings.
   - Determine whether files need to be joined. Verify join keys and report row counts before and after joining; ensure the join did not unexpectedly duplicate or drop records.
   - If required fields, promotion coding, or comparison units are ambiguous, explain the issue and ask for clarification rather than guessing.

2. **Preserve original data.**
   - Treat original CSV files as read-only. Do not overwrite or modify them.
   - Perform analysis on in-memory data or a separate derived object.
   - Do not fabricate, infer, or silently impute data or results.

3. **Evaluate each promotion independently.**
   - Analyze **Email Promotion** and **Homepage Promotion** separately.
   - For each promotion type, define the promoted and non-promoted groups using the verified field values. Document how missing, ambiguous, or invalid promotion values are handled.
   - Do not combine the two promotion types into one exposure. If both promotions occur together, report their overlap or explain how it affects interpretation of each separate comparison.

4. **Construct matched comparisons.**
   - Compare orders for the same meal at the same fulfillment center across weeks with the selected promotion and weeks without it.
   - Use verified identifiers for meal, fulfillment center, and week.
   - Confirm observations represent comparable weekly units and that demand is measured consistently.
   - Inspect repeated, missing, or unbalanced observations. Do not treat unmatched records as matched comparisons.
   - State whether the analysis compares all eligible weekly observations or restricts to meal-center units that have observations in both promotion groups. Document exclusions and the resulting scope.

5. **Check sample sizes and data suitability.**
   - Report the number of observations in each group and the number of meal-center comparison units represented.
   - Check for missing or non-finite order values, extremely small groups, ties, and duplicated or dependent observations.
   - If either group is empty, the sample is too small for a useful comparison, or the observations do not support the requested test, do not report an inferential result as reliable. Explain the limitation and report only appropriate descriptive results.
   - Consider that weekly observations for the same meal-center unit may not be independent. The ordinary Mann–Whitney U test does not account for clustering, repeated measures, seasonality, or other confounders. Do not present its p-value as resolving these limitations.

6. **Calculate descriptive comparisons.**
   - Calculate the arithmetic mean of `num_orders` separately for promoted and non-promoted observations for each promotion type, using the stated analysis scope.
   - Calculate the percent difference relative to the non-promoted mean as:

     `((promoted_mean - non_promoted_mean) / non_promoted_mean) * 100`

   - State that a positive value means the promoted-group mean is higher and a negative value means it is lower. If the non-promoted mean is zero, the percent difference is undefined; report it as not calculable rather than dividing by zero.
   - Include the group sizes and units/scope alongside means and percent differences.

7. **Apply Mann–Whitney U when appropriate.**
   - Use a two-sided Mann–Whitney U test to compare the distributions of order counts between the promoted and non-promoted groups for each promotion type only when both groups contain suitable independent observations.
   - State the test alternative, significance threshold if one is used, and how ties or missing values were handled.
   - Mann–Whitney U tests whether the groups' distributions differ; it is not generally a test of means. Do not interpret a significant result as proof that the medians differ unless the additional distribution-shape assumptions support that interpretation.
   - Explain that the test assumes independent observations and that matched/repeated meal-center observations, clustering, seasonality, confounding, unequal distribution shapes, and selection effects may limit inference. If independence is not credible, present the test only as an exploratory comparison with an explicit caveat, or do not use it for inferential claims.

8. **Interpret and present results carefully.**
   - Present Email Promotion and Homepage Promotion in separate result sections or table rows.
   - For each, report group definitions, scope, sample sizes, group means, percent difference and formula, Mann–Whitney U statistic and p-value when appropriate, and any exclusions or limitations.
   - Explain that a p-value is the probability, under the test's null model and assumptions, of observing a result at least as extreme as the one obtained. It is not the probability that the null hypothesis is true, nor a measure of effect size or practical importance.
   - If comparing two promotion types, note that multiple testing can increase false-positive risk; state whether any correction was applied. Do not add a correction silently.
   - A statistically significant association does not show that a promotion caused demand to change. Other factors, including seasonality, price, meal characteristics, availability, fulfillment-center differences, and simultaneous promotions, may explain observed differences.
   - Never invent results. If data quality, sample size, or design prevents a reliable conclusion, say so directly and distinguish descriptive patterns from inferential evidence.
