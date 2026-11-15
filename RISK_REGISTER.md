# Risk Register

| # | Risk | Likelihood | Impact | Mitigation / owner |
|---|---|---|---|---|
| 1 | Tiny sample overstates accuracy | High | High | Do not deploy ML; collect more data. Data lead |
| 2 | Leakage via last_login_days | Medium | High | Confirm measurement date; freeze features at prediction date |
| 3 | Plan type proxies affordability | High | Medium | Fairness report by plan; no price-based denial |
| 4 | Undocumented consent | Medium | High | Verify consent before real use. Compliance |
| 5 | Drift after launch | Medium | Medium | Monthly monitoring; rollback triggers in memo |
| 6 | Misuse of scores against customers | Low | High | Intended-use ban; human review for edge cases |
