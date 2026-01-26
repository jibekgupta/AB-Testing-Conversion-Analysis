# A/B Testing: Landing Page Conversion Analysis

## Overview
This project evaluates whether a new landing page improves user conversion compared to an existing page using a controlled A/B experiment. The analysis follows a full end-to-end experimentation workflow, from data validation to statistical testing and business recommendations.

---

## Dataset
**Columns:**

```user_id, timestamp, group (control/treatment), landing_page (old/new), converted (0/1)```

---

## Objective
Determine whether the new landing page:
1. Performs better than the old page
2. Shows statistically significant improvement
3. Delivers practically meaningful business impact

---

## Methodology
- **Data validation:** schema checks, missing values, duplicate user removal
- Experiment integrity:** group–page consistency checks and randomization balance
- **EDA:** overall and group-wise conversion rates with visual comparison
- **Statistical testing:** two-proportion Z-test at α = 0.05
- **Effect size:** absolute lift and relative lift
- **Uncertainty:** 95% confidence interval for conversion rate difference

---

## Key Results
- Identified the better-performing landing page based on observed conversion rates
- Assessed statistical significance using a two-tailed hypothesis test
- Quantified practical impact using absolute and relative lift
- Estimated uncertainty bounds using a 95% confidence interval

---

## Business Recommendation
- Roll out the better-performing page only if both statistical and practical significance criteria are met
- Use confidence interval bounds to evaluate best- and worst-case outcomes
- If lift is marginal, extend the experiment or test stronger variants

---

## Tools & Techniques
Python · Pandas · NumPy · Matplotlib · Statistical Hypothesis Testing · A/B Testing · Experimental Design · Data Analysis

---

## Reproducibility
The notebook is fully reproducible and modular, with clear separation of:
- data cleaning
- analysis
- statistical testing
- decision logic













