# Goal — Losing Female Signing Auditors and Corporate Tax Avoidance

## Research question (one sentence)

Does the loss of an all-female signing-auditor pair causally raise reported book-tax differences at Chinese A-share listed firms?

## What I want to know (3 bullets, plain English)

- When a firm replaces its all-female (FF) signing-auditor pair with any non-FF combination, does book-tax difference (BTD) rise, and by how much?
- Is the effect driven by mandatory partner rotation (a clean quasi-experimental slice) or by voluntary auditor change?
- Is the effect symmetric (does gaining an FF pair lower BTD) or asymmetric (only loss matters)?

## Data I have

- Source: CSMAR — financial statements, auditor and signing-CPA data, ownership data.
- Unit of observation: firm-year.
- Time coverage: 2017-2024.
- Sample size: 20,306 firm-years, 3,381 firms, 18 industries, 31 provinces, 84 audit firms.
- Key variables: BTD (winsorized 1/99), CETR (symmetric winsorization 1/99), signing-CPA gender pair (FF, MM, FM/MF), Big-4 indicator, SOE indicator, size, leverage, ROA, CFO gender, audit-firm tenure, audit fee, firm age.

## Why this matters

The auditor's role in disciplining tax positions is established at the firm level (Big-4 monitoring, partner tax expertise). The individual-auditor demographic channel — distinct from expertise — has no clean causal identification in the published literature. China's dual-signature requirement plus 5-year mandatory partner rotation make this question answerable through a within-firm event study with placebo-tested asymmetry.

## Hypotheses (pre-specified at Phase 0; resolved at Phase 5)

- H1 (main): FF-loss raises BTD at t=0. Predicted beta > 0.
  - **Supported (Phase 5).** Beta = +0.0089 (SE 0.0032, p = 0.006), N = 20,306.
- H2 (persistence): Effect persists at t=+1.
  - **Supported.** Beta = +0.0083 (SE 0.0035, p = 0.018).
- H3 (specificity): Effect is absent for MM-loss to mixed (falsification).
  - **Supported.** MM -> Mix beta = +0.002 (p = 0.39).
- H4 (asymmetry): Gaining an FF pair (FM/MF -> FF) does not lower BTD by an equivalent magnitude.
  - **Supported (binding falsification).** Gain arm beta = +0.0012 (p = 0.61), economically and statistically null.
- H5 (mandatory rotation): Effect survives in the within-office partner-rotation subsample, which proxies for PRC 5-year mandatory rotation.
  - **Rejected (Phase 5).** Headline coefficient is significant in the subsample, but HonestDiD (Rambachan-Roth 2023) shows the 95% CI crosses zero at M-bar ≈ 0.002. Demoted to corroborative descriptive evidence; full M-bar curve reported in the appendix.

## Locked direction (Phase 5)

The paper studies whether the within-firm dissolution of an all-female signing-auditor pair raises reported book-tax differences at Chinese A-share firms. The identification exploits within-firm transitions from FF to non-FF pairs. The headline coefficient is +0.0089 (SE 0.0032, p = 0.006, N = 20,306), representing about 25 percent of the BTD sample mean. The binding falsification is the null gain arm (FM/MF -> FF, beta = +0.0012, p = 0.61), ruling out symmetric mean reversion. Parallel trends survive Rambachan-Roth sensitivity through M-bar ≈ 0.002 in the main spec; the PRC mandatory rotation subsample is reported as corroboration and does not survive M-bar = 0.
