# Causal Inference — Workforce Attrition

## Overview

This project investigates whether working overtime **causally increases** 
the likelihood of employee attrition using observational HR data. Rather 
than relying on a simple group comparison, this analysis applies 
**propensity score matching** to construct comparable treatment and control 
groups — enabling a causal interpretation of the observed effect.

---

## Business Question

> Does requiring employees to work overtime causally increase their 
> likelihood of leaving the organization?

Understanding the causal drivers of attrition is critical for HR 
decision-making. Correlation alone is insufficient — employees who work 
overtime may differ from those who do not in ways that independently 
influence attrition. This project addresses that challenge directly.

---

## Why Propensity Score Matching

The IBM HR Analytics dataset is observational, not experimental. Employees 
are not randomly assigned to overtime — they differ systematically in age, 
income, job level, tenure, and other characteristics. A naive comparison 
of attrition rates between overtime and non-overtime employees would 
conflate the effect of overtime with these underlying differences.

Propensity score matching solves this by pairing each overtime employee 
with a non-overtime employee who has a similar probability of working 
overtime given their observed characteristics. This produces balanced 
groups that support a valid causal comparison.

---

## Dataset

**IBM HR Analytics Employee Attrition & Performance**  
Source: [Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)  
1,470 employee records | 35 features

> Download `WA_Fn-UseC_-HR-Employee-Attrition.csv` and place it in the 
> `data/` folder before running the notebooks.

---

## Project Structure

    causal-inference-workforce-attrition/
    │
    ├── data/                               # Raw and processed data (not tracked)
    ├── notebooks/
    │   ├── 01_eda.ipynb                    # Exploratory data analysis
    │   ├── 02_propensity_matching.ipynb    # Propensity score matching
    │   └── 03_analysis_and_results.ipynb  # Statistical testing and results
    ├── outputs/                            # Saved visualizations
    ├── src/
    │   └── matching.py                     # Reusable matching functions
    ├── requirements.txt
    └── README.md

---

## Methodology

### 1. Exploratory Data Analysis
- Assessed class imbalance in attrition
- Examined treatment variable distribution and raw attrition rates
- Identified covariate imbalance between overtime and non-overtime groups
- Conducted correlation analysis to inform covariate selection

### 2. Propensity Score Matching
- Estimated propensity scores using logistic regression with 13 covariates
- Applied nearest neighbor matching with a caliper of 0.2 standard 
deviations
- Refined the model with interaction terms to resolve residual imbalance
- Validated balance using KDE plots and Standardized Mean Difference (SMD) 
— all 13 covariates fell below the 0.1 SMD threshold after matching

### 3. Causal Analysis
- Compared attrition rates between matched groups
- Tested statistical significance using a chi-square test
- Quantified effect size using risk difference and relative risk
- Assessed robustness using Rosenbaum bounds sensitivity analysis

---

## Results

| Metric | Value |
|---|---|
| Attrition Rate — OverTime | 30.6% |
| Attrition Rate — No OverTime | 9.7% |
| Risk Difference | 20.9 percentage points |
| Relative Risk | 3.15x |
| Chi-Square Statistic | 54.50 |
| P-Value | < 0.001 |
| Sensitivity (Γ at p = 0.05) | ~2.5 |

![Stakeholder Summary](outputs/stakeholder_summary.png)

---

## Key Findings

Employees who work overtime are **3.15 times more likely** to leave the 
organization than comparable employees who do not. This finding is 
statistically significant (p < 0.001) and remains robust to unmeasured 
confounding up to Γ = 2.5 — meaning a hidden confounder would need to 
more than double the odds of overtime assignment to explain away the 
observed effect.

---

## Recommendation

Reducing employee overtime exposure is a high-impact lever for improving 
retention. Organizations should audit which roles and departments drive 
overtime demand, assess whether staffing levels are adequate, and monitor 
attrition risk among consistently overtime-exposed employees.

---

## Setup

```bash
git clone https://github.com/Drew-Zeimetz/causal-inference-workforce-attrition.git
cd causal-inference-workforce-attrition
python -m venv venv
source venv/Scripts/activate
pip install -r requirements.txt
```

---

## Dependencies

See `requirements.txt`. Key libraries: `pandas`, `numpy`, `matplotlib`, 
`seaborn`, `scikit-learn`, `scipy`, `statsmodels`.

---