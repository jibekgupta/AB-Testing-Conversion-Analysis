# A/B Testing Project

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat&logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-1.5+-150458?style=flat&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-1.21+-013243?style=flat&logo=numpy&logoColor=white)
![matplotlib](https://img.shields.io/badge/matplotlib-3.5+-11557c?style=flat&logo=matplotlib&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat)

---

## Executive Summary

This project analyzes an A/B test comparing two landing page variants (old vs new) to determine whether the new page leads to a higher conversion rate. I used Python for data cleaning, exploratory analysis, statistical hypothesis testing, and business recommendation — delivering an end-to-end A/B testing pipeline from raw data to actionable insights.

> **Core question:** Does the newly designed landing page result in a higher conversion rate compared to the existing (old) landing page?

---

## Key Results at a Glance

| Metric | Value |
|---|---|
| Total users (cleaned) | 288,540 |
| Control conversion rate | 12.03% |
| Treatment conversion rate | 11.87% |
| Absolute lift | −0.16 pp |
| P-value (two-tailed) | 0.196 |
| Statistically significant? | No |
| Practically significant? | No (below 1 pp threshold) |
| **Recommendation** | **Retain the old landing page** |

---

## Project Workflow

```
Raw Dataset (ab_data.csv, ~294K rows × 5 cols)
        │
        ▼
┌─────────────────────────┐
│  Python (Pandas/NumPy)  │  → Load, clean, validate
│  Jupyter Notebook       │  → Remove duplicates, filter group–page mismatches
└─────────────────────────┘
        │
        ▼
┌─────────────────────────┐
│  EDA & Design Check     │  → Conversion by group, balance, MDE
│  Experimental Validation │  → Randomization, independence, sample size
└─────────────────────────┘
        │
        ▼
┌─────────────────────────┐
│  Statistical Testing    │  → Two-proportion z-test, 95% CI
│  Effect Size & Robustness│  → Lift, practical significance, time/segment checks
└─────────────────────────┘
        │
        ▼
 Business Recommendation (Retain old page)
```

---

## Dataset Overview

- **Source:** A/B test user-level records (simulated real-world experiment data)
- **Records:** 294,478 raw rows → 288,540 after cleaning (duplicates and group–page mismatches removed)
- **Dimensions:** User exposure, experimental assignment, landing page variant, conversion outcome

| Column | Description |
|---|---|
| `user_id` | Unique identifier for each user |
| `timestamp` | When the user was exposed to the experiment |
| `group` | Experimental assignment (`control` or `treatment`) |
| `landing_page` | Page variant shown (`old_page` or `new_page`) |
| `converted` | Binary outcome (0 = no, 1 = yes) |

**Data quality:** 3,894 duplicate users removed (first exposure retained); 2,044 group–page mismatches removed (control must see old page, treatment must see new page).

---

## 1. Data Understanding & Cleaning

**Notebook:** `A_B_testing.ipynb` (Sections 1–3)

Steps performed:
- Loaded `ab_data.csv` and validated schema (expected columns, value domains)
- Converted `timestamp` to datetime for time-based analysis
- Identified 3,894 duplicate users — removed duplicates, keeping first exposure
- Identified 2,044 group–page mismatches (control shown new page or treatment shown old page) — filtered to retain only valid pairings
- Post-cleaning sanity check: crosstab confirmed control→old, treatment→new only; group sizes balanced (~144K each)

---

## 2. EDA & Experimental Design Validation

**Notebook:** `A_B_testing.ipynb` (Sections 4–5)

Steps performed:
- Computed overall conversion rate (~12.0%) and group-wise metrics (users, conversions, rates)
- Visualized conversion rate and user count by group (bar charts)
- Validated randomization: group proportions ~50/50, no imbalance (>55/45)
- Verified independence: no duplicate users in cleaned data
- Sample size adequacy: MDE ~0.34 pp; dataset meets 1 pp MDE requirement (n ≈ 16.6K per group needed)

---

## 3. Statistical Testing

**Notebook:** `A_B_testing.ipynb` (Sections 6–9)

Steps performed:
- Selected two-proportion z-test (binary outcome, independent groups)
- Set significance level α = 0.05
- Executed test: pooled proportion, z-score, two-tailed p-value
- Decision rule: fail to reject H₀ (p ≈ 0.20 > 0.05)
- Effect size: absolute lift −0.16 pp, relative lift −1.3%
- Practical significance: below 1 pp threshold — not practically meaningful
- 95% confidence interval for difference: (−0.39 pp, 0.08 pp) — includes 0

---

## 4. Robustness Checks & Final Recommendation

**Notebook:** `A_B_testing.ipynb` (Sections 10–12)

Steps performed:
- Daily conversion rate over time by group — checked for temporal trends
- Early stopping bias check — cumulative conversion over experiment timeline
- Segment analysis (day vs night, 6:00–18:00 = day) — conversion by time-of-day
- Final conclusion: old page performed better; not statistically or practically significant
- Business recommendation: retain old page; risk, expected impact, and next steps documented

---

## Key Business Insights

1. **Old page outperformed new page** — Control (old) had 12.03% conversion vs treatment (new) at 11.87%, a −0.16 pp difference.
2. **No statistical evidence of a real effect** — P-value 0.20 indicates the observed difference could easily be due to chance; we fail to reject the null hypothesis.
3. **Effect is not practically meaningful** — Even if real, −0.16 pp is well below a 1 pp business threshold; rollout would not justify implementation cost.
4. **95% CI includes zero** — The true effect could be no difference or even favor the new page; uncertainty is high.
5. **Robustness checks support stability** — Daily trends, cumulative conversion, and day/night segments did not reveal anomalies that would change the conclusion.

---

## Business Recommendations

| Priority | Recommendation | Rationale |
|---|---|---|
| High | Retain the old landing page | No statistical or practical evidence to support rollout of the new design |
| Medium | Extend experiment if borderline lift is observed in future | Current sample was sufficient for 1 pp MDE; if new test shows borderline results, run longer |
| Medium | Monitor key metrics post-rollout for any future changes | Conversion rate, revenue, bounce rate — baseline established for future experiments |
| Low | Conduct segment-specific follow-up if needed | Day/night and time-based analysis available; targeted experiments could explore segment differences |

---

## Repository Structure

```
A_B_Testing/
│
├── A_B_testing.ipynb       # Full analysis: cleaning, EDA, testing, robustness, recommendation
├── ab_data.csv             # Raw A/B test data (~294K rows)
├── requirements.txt        # Python dependencies (pandas, numpy, matplotlib)
└── README.md               # Project documentation
```

---

## Reproducibility

**Python environment:**
```bash
pip install -r requirements.txt
```

**Run the notebook:**
```bash
# From the project root (so ab_data.csv is found)
jupyter notebook A_B_testing.ipynb
```

Or open `A_B_testing.ipynb` in Jupyter Notebook or JupyterLab and run all cells (Cell → Run All).

---

## Skills Demonstrated

`A/B Testing` · `Hypothesis Testing` · `Two-Proportion Z-Test` · `Statistical Significance` · `Practical Significance` · `Confidence Intervals` · `Effect Size` · `Data Cleaning` · `Exploratory Data Analysis` · `Experimental Design Validation` · `Robustness Checks` · `Business Recommendation` · `Data Storytelling`

---

*Analysis by Jibek Gupta · [jibekgupta059@gmail.com](mailto:jibekgupta059@gmail.com)*
