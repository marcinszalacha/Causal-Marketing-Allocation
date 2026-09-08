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

## Methodology

The project separates experimental inference, model selection and final policy evaluation.

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

### 3. Causal Machine Learning

The project compares:

- T-Learner
- X-Learner
- DR-Learner

Models are selected separately for each treatment and outcome.

### 4. Treatment-Effect Evaluation

Heterogeneous treatment-effect models are evaluated using:

- Qini / incremental-gain curves
- CATE ranking
- uplift deciles
- held-out randomized observations

### 5. Profit-Aware Policy Optimization

Estimated treatment effects on spend are combined with:

- email delivery cost
- customer-level treatment choice
- contact-budget constraints
- off-policy evaluation

The final policy can choose:

- No E-Mail
- Mens E-Mail
- Womens E-Mail

for each customer.

## Key Experimental Results

Average treatment effects relative to No E-Mail:

- Mens E-Mail increased conversion by approximately **0.68 percentage points**
- Womens E-Mail increased conversion by approximately **0.31 percentage points**
- Mens E-Mail increased spend by approximately **$0.77 per customer**
- Womens E-Mail increased spend by approximately **$0.42 per customer**

## Causal Model Findings

The selected heterogeneous treatment-effect models showed mixed out-of-sample stability.

For conversion uplift, selected models produced negative final-test Qini scores, indicating that individual-level conversion-uplift ranking did not generalize strongly.

Spend-effect ranking was also mixed across treatment arms.

This is an important result: complex causal ML models do not automatically outperform simpler experimental strategies and must be evaluated on genuinely held-out randomized data.

## Final Policy Evaluation

Using an assumed email-delivery cost of **$0.05 per customer**, the best IPS-estimated policy on the final randomized test sample was:

### Profit-Aware Unrestricted Policy

- Estimated incremental profit: approximately **$10,433**
- Final test population: **12,800 customers**
- Customers contacted: approximately **79%**

Comparison:

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

## Controlled Confounding Experiment

The project also introduces artificial treatment-selection bias to demonstrate the difference between randomized and observational causal estimation.

Approximate spend-effect estimates:

| Method | Estimated Effect |
|---|---:|
| Randomized benchmark | 0.766 |
| Naive observational estimate | 1.073 |
| Regression adjustment | 0.899 |
| IPW | 0.853 |
| Cross-fitted AIPW | 0.860 |

The naive observational estimate substantially overstated the randomized benchmark, while causal adjustment methods moved the estimate closer to the experimental effect.

## Key Takeaways

This project demonstrates the difference between:

- prediction and causality
- average treatment effects and heterogeneous treatment effects
- model selection and final evaluation
- treatment-effect estimation and policy optimization
- predicted business value and experimentally grounded off-policy evaluation

The main business conclusion is that causal policy optimization can add value beyond blanket targeting, but only when its recommendations are evaluated against simple experimental baselines using held-out randomized data.

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
