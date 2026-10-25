# Agent Skill Relevance Tests

These tests evaluated whether each project skill was suitable for the described request. Copilot confirmed that both skills were loaded through the skills tool during testing. The tests assessed intended usage only: no data analysis was performed, and the tests do not establish that either skill activates automatically for similar requests.

## `demand-kpi-calculator`

| Test | Request | Result | Reason |
|---|---|---|---|
| Relevant | Calculate total orders, average weekly orders, and revenue. | Use the skill. | The request directly matches the skill's purpose: calculating core meal-demand KPIs from project data, including total orders, weekly demand, and revenue. |
| Irrelevant | Find skills for developing website interfaces. | Do not use the skill. | Skill discovery for website development is outside the KPI calculator's scope. |

## `promotion-impact-checker`

| Test | Request | Result | Reason |
|---|---|---|---|
| Relevant | Compare demand for the same meal at the same fulfillment center during weeks with an email promotion and weeks without one. | Use the skill. | The request matches the skill's focus on comparing meal demand with and without promotions, using the same meal and fulfillment center, and considering appropriate statistical tests. |
| Irrelevant | Create a login page using HTML and CSS. | Do not use the skill. | Building a web login page is unrelated to assessing promotional patterns and meal demand. |

## Test Limitations

Copilot confirmed loading both skills through the skills tool during these tests. The tests only assessed whether each request was relevant to the skill; no project data was inspected or analyzed, no statistical test was run, and automatic activation was not demonstrated.
