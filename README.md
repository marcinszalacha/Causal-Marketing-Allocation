# Causal Marketing Allocation

## Incremental Profit Under Budget Constraints

This project develops a causal machine learning framework for marketing allocation using the randomized Hillstrom email experiment.

The goal is not simply to predict **who is most likely to purchase**, but to estimate **whose behaviour changes because of a marketing intervention** and use those treatment-effect estimates to optimize marketing allocation under campaign-cost and contact-budget constraints.

## Business Question

For each customer, the business can choose one of three actions:

- No E-Mail
- Mens E-Mail
- Womens E-Mail

The central decision problem is:

> Given a limited marketing budget, which customers should receive which treatment in order to maximize incremental profit?

## Dataset

The project uses the Hillstrom Email Marketing dataset with approximately 64,000 customers randomly assigned to:

- No E-Mail
- Mens E-Mail
- Womens E-Mail

Pre-treatment customer features include historical spend, recency, previous mens/womens purchases, geography, customer status and purchase channel.

Post-treatment outcomes include:

- Visit
- Conversion
- Spend

Only pre-treatment variables are used as model features.

Exact duplicate rows are retained because the public dataset does not include a unique customer identifier; identical observed records may therefore represent distinct customers.

## Methodology

The project separates experimental inference, causal model selection and final policy evaluation.

### 1. Randomized Experiment Analysis

- Validated treatment-group balance
- Checked standardized mean differences
- Estimated Average Treatment Effects
- Calculated uncertainty intervals

### 2. Train / Validation / Final-Test Design

The data is split into:

- 60% training
- 20% validation
- 20% final test

The validation set is used for causal learner selection.

The final test set is excluded from model fitting and model selection and is used for final policy evaluation.

### 3. Predictive Baseline

A conventional response model is used as a benchmark to illustrate the difference between:

> Who is likely to convert?

and:

> Who is likely to change behaviour because of treatment?

This provides a non-causal baseline against which causal targeting strategies can be compared.

### 4. Causal Machine Learning

The project compares:

- T-Learner
- X-Learner
- DR-Learner

Models are selected separately for each treatment and outcome.

### 5. Treatment-Effect Evaluation

Heterogeneous treatment-effect models are evaluated using:

- Qini / incremental-gain curves
- CATE ranking
- uplift deciles
- held-out randomized observations

### 6. Profit-Aware Policy Optimization

Estimated treatment effects on spend are combined with:

- email-delivery cost
- customer-level treatment choice
- contact-budget constraints
- off-policy evaluation

The final policy can choose:

- No E-Mail
- Mens E-Mail
- Womens E-Mail

for each customer.

### 7. Controlled Confounding Stress Test

The project also deliberately introduces treatment-selection bias to create an observational-style setting.

Several causal estimators are then compared against the randomized experimental benchmark:

- Naive observational difference
- Regression adjustment
- Inverse Probability Weighting
- Cross-fitted AIPW

This demonstrates how causal adjustment methods behave when randomization is no longer available.

## Key Experimental Results

Average treatment effects relative to No E-Mail:

- Mens E-Mail increased conversion by approximately **0.68 percentage points**
- Womens E-Mail increased conversion by approximately **0.31 percentage points**
- Mens E-Mail increased spend by approximately **$0.77 per customer**
- Womens E-Mail increased spend by approximately **$0.42 per customer**

These experimental results indicate that both email treatments generated positive average causal effects, with Mens E-Mail producing the larger average effect.

## Causal Model Findings

The selected heterogeneous treatment-effect models showed mixed out-of-sample stability.

For conversion uplift:

- Mens DR-Learner final-test Qini: approximately **-0.000492**
- Womens X-Learner final-test Qini: approximately **-0.000339**

These negative final-test Qini values indicate that individual-level conversion-uplift ranking did not generalize strongly out of sample.

Spend-effect ranking was also mixed:

- Mens DR-Learner validation incremental-value score: approximately **-0.0080**
- Mens DR-Learner final-test incremental-value score: approximately **0.0357**
- Womens X-Learner validation incremental-value score: approximately **0.0653**
- Womens X-Learner final-test incremental-value score: approximately **-0.0224**

This is an important result: complex causal ML models do not automatically produce stable individual treatment-effect rankings and must be evaluated on genuinely held-out randomized data.

## Final Policy Evaluation

Using an assumed email-delivery cost of **$0.05 per customer**, the best IPS-estimated policy on the final randomized test sample was:

### Profit-Aware Unrestricted Policy

- Estimated incremental profit: approximately **$10,433**
- Final test population: **12,800 customers**
- Customers contacted: approximately **79%**

### Policy Comparison

| Policy | Estimated Incremental Profit |
|---|---:|
| Profit-Aware Unrestricted | ~$10,433 |
| Mens Email Everyone | ~$9,395 |
| Womens Email Everyone | ~$8,862 |
| Profit-Aware Top 50% | ~$5,873 |
| Conversion-CATE Top 20% | ~$1,378 |
| Profit-Aware Top 20% | ~$873 |
| Response-Model Top 20% | ~$484 |
| Treat None | $0 |

The unrestricted profit-aware policy produced a bootstrap mean incremental profit of approximately **$10,369**, with a 95% bootstrap interval of approximately **$3,224 to $17,537**.

The result does not imply that more complex causal models are always better.

Instead, it shows that combining estimated incremental spend with explicit treatment costs can produce a useful allocation rule, while individual treatment-effect ranking itself may remain uncertain.

## Controlled Confounding Experiment

The controlled-confounding experiment demonstrates how observational treatment selection can bias causal estimates.

Approximate spend-effect estimates:

| Method | Estimated Effect |
|---|---:|
| Randomized benchmark | 0.766 |
| Naive observational estimate | 1.073 |
| Regression adjustment | 0.899 |
| IPW | 0.853 |
| Cross-fitted AIPW | 0.860 |

The naive observational estimate substantially overstated the randomized benchmark.

Regression adjustment, inverse-propensity weighting and cross-fitted AIPW all moved the estimate closer to the experimental effect.

## Key Takeaways

This project demonstrates the difference between:

- prediction and causality
- average treatment effects and heterogeneous treatment effects
- model selection and final evaluation
- treatment-effect estimation and policy optimization
- predicted business value and experimentally grounded off-policy evaluation

The main business conclusion is that causal policy optimization can add value beyond blanket targeting, but only when recommendations are evaluated against simple experimental baselines using held-out randomized data.

## Tools

- Python
- pandas
- NumPy
- scikit-learn
- EconML
- SciPy
- matplotlib
- causal inference
- uplift modelling
- policy evaluation
- bootstrap uncertainty estimation

## Repository Structure

```text
Causal-Marketing-Allocation/
├── README.md
└── causal_marketing_allocation.ipynb
```

## Limitations

- The Hillstrom experiment is historical and may not reflect current digital-marketing behaviour.
- Campaign delivery cost is assumed rather than observed in the dataset.
- Individual treatment effects are not directly observable.
- Heterogeneous treatment-effect estimates are model-dependent.
- The final test evaluates generalisation within the same historical experimental population.
- Long-term customer lifetime value is not available.
- The controlled-confounding experiment is semi-synthetic and is used for methodological demonstration.

## Project Purpose

This project was built as a portfolio demonstration of applied causal machine learning, moving beyond conventional response prediction toward treatment-effect estimation and business decision optimization.
