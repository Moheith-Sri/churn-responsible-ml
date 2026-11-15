# ML Problem-Framing Memo

| | |
|---|---|
| Decision | Which subscribers receive a retention outreach (call or small discount) in the next cycle |
| Prediction target | `churned`: 1 if the customer left within the observation window, else 0 |
| Unit of observation | One customer (`customer_id`), one snapshot |
| Action window | Features measured up to the prediction date; outreach within 14 days; outcome read after the next billing cycle |
| Non-ML baseline | Flag if `last_login_days >= 10` or `plan_type == Basic` |

## Is ML justified?
**Not yet.** The rule separates churners perfectly on 12 rows (5/5 caught, 0 false alarms); logistic regression only matches it (leave-one-out: 1 false positive, 0 misses). Any gain from ML cannot be distinguished from noise at this size. Deploy the rule, log outcomes, and revisit ML with several hundred labeled, time-stamped customers.

## Metrics and error costs (assumptions; replace with finance data)
- False negative (missed churner): about INR 4,800 (about 6 months of average spend)
- False positive (needless outreach): about INR 150
- Outreach saves 30% of true churners
- Primary model metric: recall on churners (a miss costs about 32x a false alarm). Secondary: precision, Brier score. Business metric: net retention savings (INR).

## Abstention, human review, monitoring, rollback
- **Abstain / human review:** under 3 months of history, missing fields, or model probability 0.35 to 0.65. No automated account action beyond sending an offer.
- **Monitor monthly:** churn rate, recall, outreach rate by plan, feature drift (login days, spend), calibration.
- **Rollback to the rule if:** recall falls below 0.70, outreach cost exceeds saved revenue for 2 consecutive months, or the error-rate gap across plans exceeds 15 points.
