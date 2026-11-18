# A/B Test Analysis

**Maksym Voronenko | Data Analyst**

Analysis of four A/B experiments using SQL, Python, and Tableau. The project compares checkout events and account-linked sessions between control group A and variant group B, applies statistical tests, and adjusts p-values for multiple comparisons.

**Tools:** SQL, Google BigQuery, Python, pandas, NumPy, SciPy, statsmodels, Tableau.

[View the notebook](Project_2_AB_Testing.ipynb) · [Open in Google Colab](https://colab.research.google.com/drive/10Pod_pMy0RahVPuJqCVPlZJlrj4iIR6l)

## Objective

Evaluate differences between A and B across four metrics in each experiment: **4 experiments × 4 metrics = 16 comparisons**. Group 1 is control A; group 2 is variant B. All changes are reported as B versus A.

## Data and Metrics

The notebook queries the `data-analytics-mate` BigQuery project and its `DA` dataset. SQL joins experiment assignments, sessions, session attributes, events, orders, and account-session records, then aggregates the data for analysis.

| Metric | Definition used in the notebook |
| --- | --- |
| Payment information | `add_payment_info` event count / session count |
| Shipping information | `add_shipping_info` event count / session count |
| Begin checkout | `begin_checkout` event count / session count |
| New accounts | Distinct account-linked sessions / session count |

Event metrics can include repeated events within a session. The metric labelled **New accounts** counts distinct sessions found in `account_session`.

## Methodology

1. Extract and aggregate experiment data in BigQuery using SQL and common table expressions.
2. Summarize session and metric counts for each experiment and group.
3. Calculate pooled metric ratios and relative changes between A and B.
4. Apply two-sided paired t-tests to daily event/session ratios on matching dates for the three event metrics. Each matched day has equal weight.
5. Apply a two-sided two-proportion z-test to the pooled shares of account-linked sessions.
6. Apply a Bonferroni adjustment across the **four metrics within each experiment**, with a significance threshold of **0.05**.
7. Export the results as `ab_test_results.csv` from Colab for reporting in Tableau.

Pooled ratios describe overall observed changes. The event t-tests use mean daily ratios, which can differ from pooled ratios; the notebook retains both measures.

## Results

The saved notebook outputs show **no statistically significant differences in any of the 16 comparisons after the Bonferroni adjustment**.

For example, the account-linked session share in Test 4 is **8.55% for A** and **8.26% for B**. The raw p-value is **0.017527**, while the adjusted p-value is **0.070109**, above the 0.05 threshold.

These results do not establish that A and B are equivalent. Observed changes remain descriptive when the adjusted test is not significant.

## Tableau Dashboard

### Experiment Overview

Compare experiment volumes, pooled metrics, and observed changes. Use the experiment and metric controls to explore the results.

[Open Experiment Overview](https://public.tableau.com/app/profile/maxim.voronenko2142/viz/PROJECT02_17947675861610/ExperimentOverview)

![A/B testing experiment overview](https://public.tableau.com/static/images/PR/PROJECT02_17947675861610/ExperimentOverview/1.png)

### Statistical Evidence

Review the estimates used in the tests, raw and adjusted p-values, and significance decisions.

[Open Statistical Evidence](https://public.tableau.com/app/profile/maxim.voronenko2142/viz/PROJECT02_17947675861610/Statisticalevidence)

![A/B testing statistical evidence](https://public.tableau.com/static/images/PR/PROJECT02_17947675861610/Statisticalevidence/1.png)

The images are static previews hosted by Tableau Public. Open the dashboard links for interactive views.

## Repository Files

| File | Contents |
| --- | --- |
| [Project_2_AB_Testing.ipynb](Project_2_AB_Testing.ipynb) | Original notebook with SQL, Python analysis, saved tables, statistical tests, and CSV export |
| [README.md](README.md) | Project overview, methods, results, and dashboard links |

## Viewing and Running

Open the notebook on GitHub to inspect the code and saved outputs, or open the linked Google Colab notebook to run it.

Recomputing the analysis requires Google authentication and permission to query the `data-analytics-mate` BigQuery project and the referenced `DA` tables. The first notebook cell installs the required Python packages. Run the cells in their original order; the final export cell downloads `ab_test_results.csv`.

The original notebook and its saved outputs are included unchanged. No source dataset or exported CSV is bundled with this repository.
