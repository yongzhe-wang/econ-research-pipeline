# Headline Results — FF-Loss and Tax Avoidance

Locked at Phase 5. This is the one-page summary that the paper's intro and abstract pull from.

## Sample

- Universe: Chinese A-share listed firms, Shanghai + Shenzhen Stock Exchanges, 2017-2024.
- Source: CSMAR.
- Analytic sample: 20,306 firm-years, 3,381 firms, 18 industries, 31 provinces, 84 audit firms.
- Mean firm coverage: 7.45 years (min 3, max 9).
- First-event FF-loss sample: 2,589 firm-years from 751 first switches.

## Locked direction

The paper documents that the within-firm dissolution of an all-female (FF) signing-auditor pair is associated with a rise in book-tax differences (BTD) at Chinese A-share firms. The asymmetric falsification (FF-gain null on BTD) rules out symmetric mean reversion.

## Headline coefficient

beta_{FFLoss} = +0.0089 (SE 0.0032, p = 0.006, N = 20,306)

- Spec: Y = BTD; FE = firm + industry x year + province x year; controls = size, leverage, ROA, age-in-panel, Tobin's Q; SE clustered at firm level.
- Magnitude: about 25 percent of the BTD sample mean (mean BTD = 0.036).
- Stability: coefficient within +/- 0.0008 across four FE specifications (firm + year; firm + industry-year; firm + industry-year + province-year; firm + industry-year + province-year + firm-level controls).

## Binding falsification — asymmetric gain arm

beta_{FFGain} = +0.0012 (SE 0.0024, p = 0.61, N = 19,841)

- Falsification: if the headline were driven by mechanical mean reversion at any auditor switch, the gain arm should produce a coefficient of opposite sign and similar magnitude. It does not. The gain arm is statistically and economically null. This rules out symmetric mean reversion as the explanation.

## Specificity check — MM-to-mixed null

beta_{MM_to_Mix} = +0.002 (p = 0.39)

- An MM-to-mixed switch is also a "stable pair lost" event but does not involve loss of female signers. The null rules out a generic "any-stable-pair-loss" shock.

## HonestDiD (Rambachan-Roth 2023) — main spec

| M-bar | 95% CI lower | 95% CI upper |
|-------|--------------|--------------|
| 0 | +0.0024 | +0.0154 |
| 0.5 | +0.0011 | +0.0167 |
| 1.0 | -0.0003 | +0.0181 |
| 1.5 | -0.0017 | +0.0195 |
| 2.0 | -0.0031 | +0.0209 |

- Main spec parallel trends survive significance through M-bar ~ 0.95.
- CI first crosses zero at M-bar ~ 0.95-1.0.
- Interpretation: result robust to modest pre-trend violations; not robust to large violations.

## HonestDiD — PRC rotation subsample (demoted)

- PRC mandatory rotation subsample: N = 513 first events.
- Headline coefficient in subsample: +0.0072 (SE 0.0038, p = 0.058).
- HonestDiD CI crosses zero at M-bar ~ 0.002.
- Verdict: does not survive parallel-trends sensitivity at any meaningful M-bar. Demoted from headline to corroborative descriptive evidence. Full curve reported in appendix.

## Mechanism — silent monitoring (not fee-priced)

- Mediation through ln(audit_fee): 8.4% of the total effect, p = 0.41 (Sobel test).
- Audit-fee shock as outcome: null on FF-loss (beta = -0.012, p = 0.71).
- Audit opinion modification as outcome: null on FF-loss (beta = +0.003, p = 0.55).
- Interpretation: the headline channel is consistent with silent monitoring — female partners exert monitoring discipline that is not visibly priced through audit fees or formally signaled through opinion modifications. The effect operates net of these visible channels.

## Whitespace confirmation

A 7-layer search across NBER, SSRN, top-5 econ journals, top accounting/finance journals (JAE, JAR, TAR, JFE, RFS, JF), Chinese CNKI, citation graph traversal, and Codex cross-model check confirmed that:
- No published paper identifies the individual-auditor gender-pair channel within firm using Chinese A-share data with an asymmetric-arm falsification.
- The closest precedents are firm-level Big-4-monitoring papers and cross-sectional auditor-gender correlations; neither identifies the within-firm channel with the falsification design.

## What this implies for the paper

- Headline: within-firm event study (FFLoss on BTD), with the asymmetric gain arm as binding falsification.
- Corroboration: PRC rotation subsample as descriptive evidence (not causal headline).
- Mechanism: silent monitoring net of visible audit channels.
- Contribution: (a) individual-auditor demographic channel distinct from expertise, (b) within-firm event study with asymmetric falsification, (c) silent-monitoring mechanism distinct from fee or opinion channels.

## What this does NOT support

- A general claim that female auditors monitor more (the cross-section is selection; do not claim it as causal).
- A regulatory-rotation argument as the headline causal slice (HonestDiD demoted it).
- A fee-pricing mechanism (mediation null).
