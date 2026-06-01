# Error Analysis

Final model threshold: **0.24**. Test ROC-AUC: **0.885**; test recall: **0.958**; test precision: **0.665**.

## Business Meaning of Errors

- **False positives**: customers predicted as churn risks who actually purchased. Business risk is wasted incentive spend or unnecessary contact. Low-cost channels and holdouts reduce this risk.
- **False negatives**: customers predicted as lower risk who actually churned. Business risk is missed revenue retention. Monitor this group closely, especially customers with sparse history or recent disengagement.

## Specific Test Examples

| error_type | customer_id | churn_probability | predicted_churn | churn_next_60d | recency_days | frequency_180d | monetary_180d | return_rate_180d | avg_discount_pct_180d | ticket_count_90d | negative_ticket_rate_90d | sessions_30d | campaign_clicks_30d | last_visit_days_ago | loyalty_tier | marketing_consent |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| False Positive | CUST01246 | 0.982 | 1 | 0 | 262 | 0 | 0.00 | 0.000 | 0.00 | 0 | 0.000 | 1 | 3 | 60 | Silver | Yes |
| False Positive | CUST01325 | 0.957 | 1 | 0 | 186 | 0 | 0.00 | 0.000 | 0.00 | 0 | 0.000 | 1 | 0 | 43 |  | Yes |
| False Positive | CUST01411 | 0.937 | 1 | 0 | 183 | 0 | 0.00 | 0.000 | 0.00 | 0 | 0.000 | 0 | 0 | 51 |  | No |
| False Positive | CUST00437 | 0.935 | 1 | 0 | 151 | 1 | 729.22 | 0.000 | 0.25 | 0 | 0.000 | 0 | 0 | 33 | Silver | Yes |
| False Positive | CUST01370 | 0.891 | 1 | 0 | 161 | 2 | 1,246.04 | 0.000 | 0.21 | 0 | 0.000 | 2 | 0 | 35 |  | No |
| False Positive | CUST01017 | 0.888 | 1 | 0 | 133 | 2 | 1,167.28 | 0.500 | 0.23 | 0 | 0.000 | 3 | 0 | 13 |  | Yes |
| False Negative | CUST02072 | 0.047 | 0 | 1 | 35 | 7 | 4,340.19 | 0.000 | 0.42 | 0 | 0.000 | 4 | 0 | 1 |  | Yes |
| False Negative | CUST00184 | 0.068 | 0 | 1 | 14 | 3 | 2,456.91 | 0.000 | 0.29 | 0 | 0.000 | 6 | 0 | 6 | Platinum | No |
| False Negative | CUST01990 | 0.084 | 0 | 1 | 59 | 4 | 3,877.77 | 0.000 | 0.23 | 0 | 0.000 | 11 | 1 | 7 | Silver | Yes |
| False Negative | CUST00866 | 0.117 | 0 | 1 | 26 | 1 | 1,280.71 | 0.000 | 0.06 | 0 | 0.000 | 5 | 0 | 1 |  | No |
| False Negative | CUST01303 | 0.149 | 0 | 1 | 20 | 1 | 844.74 | 0.000 | 0.18 | 1 | 0.000 | 3 | 0 | 0 |  | Yes |
| False Negative | CUST01655 | 0.169 | 0 | 1 | 13 | 2 | 1,358.99 | 0.000 | 0.15 | 0 | 0.000 | 2 | 0 | 7 |  | No |

## Case Interpretation

- `CUST01246` (False Positive, p=0.982): stale order recency (262 days); low recent frequency (0); no recent visit for 60 days. The model may have over-weighted risk signals; use a low-cost message rather than an expensive discount.
- `CUST01325` (False Positive, p=0.957): stale order recency (186 days); low recent frequency (0); no recent visit for 43 days. The model may have over-weighted risk signals; use a low-cost message rather than an expensive discount.
- `CUST01411` (False Positive, p=0.937): stale order recency (183 days); low recent frequency (0); no recent visit for 51 days. The model may have over-weighted risk signals; use a low-cost message rather than an expensive discount.
- `CUST00437` (False Positive, p=0.935): stale order recency (151 days); low recent frequency (1); no recent visit for 33 days. The model may have over-weighted risk signals; use a low-cost message rather than an expensive discount.
- `CUST01370` (False Positive, p=0.891): stale order recency (161 days); no recent visit for 35 days. The model may have over-weighted risk signals; use a low-cost message rather than an expensive discount.
- `CUST01017` (False Positive, p=0.888): stale order recency (133 days); return rate 50%. The model may have over-weighted risk signals; use a low-cost message rather than an expensive discount.
- `CUST02072` (False Negative, p=0.047): high recent spend INR 4,340. The model under-called risk; this pattern should be monitored for threshold or feature updates.
- `CUST00184` (False Negative, p=0.068): high recent spend INR 2,457. The model under-called risk; this pattern should be monitored for threshold or feature updates.
- `CUST01990` (False Negative, p=0.084): strong recent activity (11 sessions); high recent spend INR 3,878. The model under-called risk; this pattern should be monitored for threshold or feature updates.
- `CUST00866` (False Negative, p=0.117): low recent frequency (1). The model under-called risk; this pattern should be monitored for threshold or feature updates.
- `CUST01303` (False Negative, p=0.149): low recent frequency (1); 1 support ticket(s). The model under-called risk; this pattern should be monitored for threshold or feature updates.
- `CUST01655` (False Negative, p=0.169): mixed mid-range signals. The model under-called risk; this pattern should be monitored for threshold or feature updates.
