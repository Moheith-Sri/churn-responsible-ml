# Responsible Data Card

## Dataset purpose
May support: ranking customers for retention outreach. Must not support: pricing discrimination, denial of service or support, credit decisions, or any individual decision without human review.

## Provenance and permission
Small training extract supplied with the task. Collection method, date range, owner, and customer consent for analytics are **not documented**. Treat as unverified; confirm consent and licensing with the data owner before real use. Currency: INR.

## Population and representation
12 customers: Basic 5, Standard 4, Pro 3. Churn 5/12 (42%), unusually high, so likely a curated sample. No region, age, or segment data, so other groups cannot be assessed. Small, possibly non-random.

## Features and target
| Field | Meaning | Risk |
|---|---|---|
| customer_id | Identifier | Exclude from model; pseudonymize before sharing |
| tenure_months | Months as customer | Low; churners avg 4.2 vs 17.9 |
| support_tickets | Tickets raised | Low if counted before prediction date |
| monthly_spend_inr | Monthly bill | Near-duplicate of plan; proxy for affordability |
| last_login_days | Days since last login | **Leakage risk** if measured after exit |
| plan_type | Basic / Standard / Pro | Perfectly separates labels (Basic = 100% churn); likely overfits |
| churned | Target (0/1) | Window definition undocumented |

No direct sensitive attributes (gender, age, religion, caste, location). Spend and plan may proxy socioeconomic status.

## Quality checks
Missing values: none. Duplicate rows/IDs: none. Class balance: 7 stayed, 5 churned. Outliers: none notable. Train/test: no hold-out possible at n=12; leave-one-out used, indicative only.

## Risks and safeguards
| Risk | Safeguard |
|---|---|
| Bias: low-spend customers flagged more | Report rates by plan; human review of borderline cases |
| Privacy | Pseudonymize IDs, restrict access, state retention period |
| Misuse against likely churners | Written intended-use ban |
| False positives (INR 150 each) | Cap outreach budget |
| False negatives (INR 4,800 each) | Monitor recall |

## Intended evaluation
Baseline: the rule. Measures: recall and precision on churners, Brier score, calibration plot (when n allows), error analysis of every misclassified customer, error rates split by plan type.
