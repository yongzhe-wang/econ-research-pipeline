# Research Design — FF-Loss and Tax Avoidance

## Research question (sharpened)

Does the within-firm dissolution of an all-female (FF) signing-auditor pair causally raise reported book-tax differences (BTD)? Is the effect asymmetric across loss vs gain events?

## Conceptual framework

Two non-mutually-exclusive channels:
(a) **Monitoring conservatism.** Female engagement partners are more risk-averse and conservative; losing both female signers loosens the within-firm audit-conservatism constraint and permits more aggressive tax positions.
(b) **Auditor-specific human capital.** The FF pair has accumulated firm-specific monitoring capital that is destroyed at the partner-rotation event and cannot be instantly rebuilt by the incoming signer.

Both predict that BTD rises when an FF pair is broken; neither predicts a symmetric drop when an FF pair is acquired (the incoming pair has no firm-specific capital yet). The asymmetric loss-vs-gain pattern is the binding falsification.

## Identifying variation

Within-firm transitions from an FF signing-pair (in years t-1, t-2) to a non-FF pair (FM/MF or MM) in year t. The PRC 5-year mandatory partner-rotation rule generates a subsample of switches that are not chosen by the client firm. We approximate this subsample with **within-office partner change** events (same firm, same audit office, partner identity flips), which yields N = 513 first events.

## Main estimating equation

BTD_{i,t} = alpha_i + alpha_{j(i),t} + alpha_{p(i),t} + beta * FFLoss_{i,t} + X_{i,t}' theta + epsilon_{i,t}

Where:
- BTD_{i,t} = book-tax difference for firm i in year t, scaled by lag total assets, winsorized 1/99.
- FFLoss_{i,t} = indicator equal to one if firm i had an FF pair in {t-1, t-2} and a non-FF pair in t; first-event stacked, tau = -1 omitted.
- X_{i,t} = size, leverage, ROA, age in panel, Tobin's Q.
- alpha_i = firm FE; alpha_{j(i),t} = industry-year FE; alpha_{p(i),t} = province-year FE.
- Cluster: firm-clustered SE baseline; province x industry double-cluster in robustness; firm + auditor-office double-cluster in subsample 5.

## Sample definition

- Universe: A-share listed firms on the Shanghai and Shenzhen Stock Exchanges, 2017-2024.
- Filters:
  1. Drop firm-years missing BTD, CETR, size, leverage, ROA, firm age.
  2. Drop firm-years missing CPA signing pair.
  3. Drop financial sector.
  4. Drop year 2025 (stub year, single observation).
  5. Drop columns that are 100% constant or 100% missing.
- Final N: 20,306 firm-years, 3,381 firms (mean 7.45 years per firm; min 3, max 9).
- First-event FF-loss sample: 2,589 firm-years from 751 first switches.

## Variables (selected)

| Paper name | Construction | Role |
|------------|--------------|------|
| BTD_{i,t} | (Pre-tax income - taxable income) / lag total assets; winsorized 1/99 | Primary outcome |
| CETR^w_{i,t} | Cash taxes paid / pre-tax income; symmetric winsorization 1/99 | Secondary outcome |
| FFLoss | I{FF in t-1, t-2; non-FF in t} | Treatment |
| FFGain | I{non-FF in t-1, t-2; FF in t} | Falsification arm |
| Size | log(total assets) | Control |
| Leverage | total liabilities / total assets | Control |
| ROA | net income / total assets | Control |
| Tobin's Q | (Equity market value + total liabilities) / total assets | Control |
| Big-4 | Audit-fee-validated indicator | Control |
| SOE | State-owned-controller indicator | Heterogeneity |
| ln_audit_fee | log(audit fee) | Mechanism / control |

## Identification threats with mitigations

- Threat 1: **Endogenous timing of switch.** Firm chooses to drop FF pair anticipating tax aggression. Mitigation: restrict to within-office partner change (PRC mandatory rotation proxy, N=513); test FFGain arm.
- Threat 2: **Bundled corporate events** (CFO turnover, audit-firm change, fee shock). Mitigation: add contemporaneous CFO change, Big-4 change, fee-shock controls in {t-1, t, t+1}. Coefficient stable at +0.0089.
- Threat 3: **Mean reversion in BTD.** Top-BTD firms revert mechanically. Mitigation: pre-trend F-test passes; FFGain arm is null on BTD (rules out symmetric reversion at any switch).
- Threat 4: **"Any-stable-pair-loss" generic shock.** Mitigation: MM -> FM/MF arm is null (beta = +0.002, p = 0.39).
- Threat 5: **Contemporaneous bundled real-side events.** Mitigation: PPE growth, capex/TA, cash change, leverage change all null on FFLoss.
- Threat 6: **TWFE bias under staggered timing.** Mitigation: Callaway-Sant'Anna and Sun-Abraham robustness; Rambachan-Roth sensitivity.

## Robustness queue

1. Alternative outcomes: CETR^w, current-period ETR, AbBTD.
2. Alternative samples: drop COVID 2020-2022; drop top/bottom 1% by size; non-SOE only; non-Big-4 only.
3. Alternative FE: firm + year; firm + industry-year; firm + industry-year + province-year; firm + industry x province x year.
4. Alternative SE: firm cluster (baseline); province x industry double-cluster; wild cluster bootstrap (999 reps).
5. Heterogeneity-robust estimators: Callaway-Sant'Anna and Sun-Abraham, overlaid on the event-study plot.
6. Honest-DID (Rambachan-Roth) sensitivity at M-bar in {0, 0.5, 1, 1.5, 2}.
7. Placebo treatments: random reshuffling of FFLoss across firms (1,000 reps).
8. Placebo outcomes: PPE growth, capex/TA, cash holdings change, leverage change.

## Pre-specified hypotheses

| Coefficient | Predicted sign | Predicted magnitude | Falsification |
|-------------|----------------|---------------------|---------------|
| beta_{FFLoss} on BTD (t=0) | + | ~+0.5 to +1.5 pp of lag assets | Significant negative or zero with |t| < 1 |
| beta_{FFLoss} on BTD (t=+1) | + (persists) | ~+0.5 to +1.5 pp | Drops to zero or reverses |
| beta_{MM -> Mix} on BTD | ~0 | |b| < 0.003 | Significant positive of FF-loss magnitude |
| beta_{FM/MF -> FF} on BTD | ~0 or weakly negative | |b| < 0.005 | Significant negative of FF-loss magnitude |
| beta_{FFLoss} on placebo outcomes | ~0 | |t| < 1.5 | Any p < 0.05 rejects design |
| beta_{FFLoss} in mandatory-rotation subsample | + | ~+0.007 to +0.010 | Drops to zero or reverses |
