# Model Card: D2C Customer Churn Prediction

## 1. Model Overview

- **Model name:** D2C 60-day churn classifier
- **Model type:** Regularized logistic classifier implemented with NumPy
- **Prediction target:** `churn_next_60d = 1`, meaning no purchase in the 60 days after the 2025-09-30 snapshot date
- **Primary users:** CRM, lifecycle marketing, customer support, and retention analytics teams

## 2. Intended Use

Use the score to prioritize retention review and campaign testing. The output should help decide who needs service recovery, replenishment reminders, or margin-controlled offers.

## 3. Data Used

The model uses `rfm_modeling_snapshot.csv`, which the data dictionary identifies as a pre-snapshot modeling table. Inputs include customer profile fields, RFM behavior, return rate, discount usage, support-ticket metrics, web/app activity, and campaign/email engagement.

Excluded from model features: `customer_id`, `snapshot_date`, `churn_next_60d`, and `split`.

## 4. Leakage Prevention

- Used the provided modeling snapshot as the feature source.
- Did not use post-snapshot rows from `orders.csv`; the raw file contains 1,872 such rows for label construction only.
- Used the provided `split` column for train/validation/test separation.
- Selected the decision threshold on validation data only.

## 5. Model Approach

The baseline predicts the train-set churn prevalence for every customer. The final model standardizes numeric variables, one-hot encodes categorical variables, and trains a regularized logistic classifier. This approach is interpretable and appropriate for a first production candidate where marketing teams need feature-level explanations.

## 6. Performance

| model | split | accuracy | precision | recall | f1 | roc_auc | pr_auc |
| --- | --- | --- | --- | --- | --- | --- | --- |
| baseline | validation | 0.562 | 0.000 | 0.000 | 0.000 | 0.500 | 0.438 |
| final | validation | 0.747 | 0.648 | 0.925 | 0.762 | 0.882 | 0.867 |
| final | test | 0.738 | 0.665 | 0.958 | 0.785 | 0.885 | 0.878 |

Selected threshold: **0.24**.

## 7. Top Drivers

| feature | coefficient | direction |
| --- | --- | --- |
| recency_days | 1.76 | increases churn risk |
| monetary_180d | -0.44 | decreases churn risk |
| preferred_category=Fragrance | -0.39 | decreases churn risk |
| acquisition_channel=Organic | -0.39 | decreases churn risk |
| return_rate_180d | 0.35 | increases churn risk |
| ticket_count_90d | -0.31 | decreases churn risk |
| loyalty_tier=Platinum | -0.31 | decreases churn risk |
| negative_ticket_rate_90d | 0.31 | increases churn risk |
| avg_discount_pct_180d | 0.30 | increases churn risk |
| last_visit_days_ago | 0.28 | increases churn risk |
| preferred_category=Baby Care | 0.28 | increases churn risk |
| sessions_30d | -0.20 | decreases churn risk |

Positive coefficients increase churn risk. Negative coefficients decrease churn risk.

## 8. Limitations

- The dataset is a snapshot and may not represent future product, pricing, or campaign changes.
- Campaign history is observational and should not be interpreted as causal lift.
- The model may underperform for new customers with sparse order/support history.
- It predicts purchase absence, not the underlying reason for churn.

## 9. Ethical and Customer Risks

- Do not use scores to deny support, refunds, or fair treatment.
- Avoid over-contacting customers or pressuring customers who opted out of marketing.
- Monitor performance across city tier, age group, loyalty tier, and acquisition channel for uneven error rates.

## 10. Monitoring Needs

Track feature drift, score distribution drift, precision/recall after outcomes mature, campaign lift, opt-outs, support escalations, and calibration by segment. Retrain if performance drops, product assortment changes, campaign strategy changes, or customer acquisition mix shifts.

## 11. When Not To Use

Do not use this model for causal campaign measurement, individual eligibility for customer support, credit/financial decisions, or decisions outside D2C retention prioritization.
