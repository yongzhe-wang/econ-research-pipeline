# 06 — Robustness

Target length: **800–1,200 words** across **3–5 tables**. Robustness section exists to **kill the obvious referee comments before the first round.** Each subsection should be 1–2 paragraphs and accompany one robustness table (often a single multi-panel table covering several checks).

## Standard robustness battery for our designs

```
6.1 Alternative outcomes               (GAAP ETR, long-run cash ETR, BTD components)
6.2 Alternative samples                (entropy balance, PSM, non-SOE, drop financial-crisis years)
6.3 Alternative fixed-effect structures (firm + industry-year, firm-office + year, …)
6.4 Alternative SE clustering          (firm; province; firm + province-year; wild cluster bootstrap)
6.5 Placebo treatments                 (fake treatment dates, fake treatment groups)
6.6 Heterogeneity-robust estimators    (Callaway-Sant'Anna; Sun-Abraham; Borusyak-Jaravel-Spiess)
6.7 Pre-trends + sensitivity analysis  (Rambachan-Roth honestDiD)
6.8 Outliers and winsorization         (5/95, drop top/bottom 1%, no winsorization)
```

Order is not sacred, but the **pre-trends + Rambachan-Roth** subsection should appear last (it is the most cited check at top-journal R&Rs).

## 6.1 — Alternative outcomes

**Skeleton paragraph.**

> Table~\ref{tab:rob-outcomes} reports estimates of
> Equation~\eqref{eq:didmain} using four alternative outcomes:
> (i) GAAP ETR; (ii) the long-run three-year cash ETR following
> \citet{dyrengbauerhanlonmaydew2008}; (iii) total BTD; (iv) the
> permanent (non-temporary) component of BTD. The treatment
> coefficient is statistically significant and of comparable
> magnitude across all four outcomes, addressing the concern that
> our findings are specific to the cash-ETR measure.

## 6.2 — Alternative samples

**Skeleton paragraph.**

> Table~\ref{tab:rob-sample} reports estimates from four alternative
> samples. Column~(1) restricts to non-SOEs; column~(2) restricts to
> firms audited by a Big-4 affiliate for the full sample; column~(3)
> uses an entropy-balanced sample matching treated and control firms
> on pre-period firm controls following \citet{hainmueller2012};
> column~(4) uses a propensity-score-matched sample (1:1 nearest
> neighbor on size, leverage, ROA, industry). The treatment
> coefficient is statistically and economically similar across
> sample restrictions, suggesting that our results are not driven by
> sample composition.

## 6.3 — Alternative fixed-effect structures

**Skeleton paragraph.**

> Table~\ref{tab:rob-fe} examines sensitivity to the fixed-effects
> structure. Column~(1) uses only firm and year fixed effects;
> column~(2) adds industry $\times$ year FE; column~(3) adds
> province $\times$ year FE (absorbing province-specific
> macroeconomic shocks and unobserved provincial policy); column~(4)
> adds firm-pair $\times$ year FE (for Design A: firm
> $\times$ auditor-office $\times$ year FE). The coefficient is
> stable across specifications, confirming that the result is not
> driven by industry- or province-time confounders.

## 6.4 — Alternative SE clustering

**Skeleton paragraph.**

> Standard errors in the main specification are two-way clustered at
> the firm and province-year levels. Table~\ref{tab:rob-se} reports
> alternative clustering choices: firm-only; province-only; firm and
> province two-way; and a wild cluster bootstrap with 999 replications
> following \citet{cameronmillerbootstrap2008}. The $t$-statistic on
> the treatment coefficient remains above <2.5> under all clustering
> choices.

## 6.5 — Placebo treatments

**Skeleton paragraph.**

> We conduct two placebo tests. First, we re-estimate
> Equation~\eqref{eq:didmain} assigning each treated firm a fake
> treatment date five years before its actual treatment date. The
> placebo coefficient is <0.00X> (<SE>), statistically
> indistinguishable from zero, supporting the parallel-trends
> interpretation. Second, we randomly reassign the treatment
> indicator across firms (preserving the cross-sectional share of
> treated firms) and re-estimate the model 1,000 times. The
> distribution of placebo coefficients is centered at zero and our
> actual coefficient lies in the <99th> percentile of the placebo
> distribution.

## 6.6 — Heterogeneity-robust estimators

**Skeleton paragraph.**

> Two-way fixed-effect DID estimators can be biased under staggered
> treatment timing and heterogeneous treatment effects
> \citep{sunabraham2021, callawaysantanna2021, borusyakjaravelspiess2024}.
> Table~\ref{tab:rob-estimators} compares our two-way fixed-effect
> estimate with three heterogeneity-robust alternatives:
> (i) the Callaway-Sant'Anna doubly-robust group-time ATT,
> aggregated to a single weighted average;
> (ii) the Sun-Abraham interaction-weighted estimator;
> (iii) the Borusyak-Jaravel-Spiess imputation estimator. All three
> point estimates lie within the 95\% confidence interval of the
> baseline, indicating that heterogeneity-driven contamination is
> not material.

## 6.7 — Pre-trends and Rambachan-Roth sensitivity

This is the **single most-asked-for robustness check** at top-journal R&Rs since 2023.

**Skeleton paragraph.**

> Figure~\ref{fig:eventstudy} demonstrates that pre-treatment
> event-time coefficients are statistically indistinguishable from
> zero (joint $F$-test $p = 0.<XX>$). To assess sensitivity to
> small deviations from parallel trends, we apply the
> \citet{rambachanroth2023} partial-identification approach.
> Figure~\ref{fig:rambachanroth} reports the robust 95\%
> confidence set for the average post-treatment effect under the
> restriction that the post-treatment violation of parallel trends
> is no larger than $\bar{M}$ times the largest pre-treatment
> violation, for $\bar{M} \in \{0, 0.5, 1, 1.5, 2\}$. Our main
> conclusion (a positive and economically meaningful treatment
> effect) survives at $\bar{M} = <1.5>$, which we view as a
> generous benchmark for plausible parallel-trend violations.

## 6.8 — Outliers and winsorization

**Skeleton paragraph.**

> Table~\ref{tab:rob-winsor} reports estimates under three
> alternative winsorization choices: no winsorization, 5/95
> winsorization, and trimming (drop) of the top and bottom 1\%.
> The coefficient is stable across all three choices, suggesting
> that our results are not driven by extreme observations.

## Robustness-table layout convention

Robustness checks typically share a **single multi-panel table**, one column per check, to save space:

```latex
\begin{table}
\caption{Robustness Checks}
\label{tab:rob}
\centering
\footnotesize
\begin{tabular}{lcccccc}
\toprule
                                & (1) Baseline & (2) Non-SOE & (3) PSM
                                & (4) C-S      & (5) Wild boot. & (6) Long-run CETR \\
\midrule
Post $\times$ Treat             & 0.024***     & 0.026***   & 0.022***
                                & 0.023***     & 0.024***       & 0.031*** \\
                                & (0.006)      & (0.007)    & (0.008)
                                & (0.007)      & [0.000]        & (0.009) \\
\addlinespace
Firm FE                         & Yes & Yes & Yes & Yes & Yes & Yes \\
Year FE                         & Yes & Yes & Yes & Yes & Yes & Yes \\
Controls                        & Yes & Yes & Yes & Yes & Yes & Yes \\
Cluster                         & Firm/PvY    & Firm/PvY   & Firm/PvY
                                & Firm/PvY    & Wild        & Firm/PvY \\
$N$                             & 26{,}811    & 14{,}320   & 19{,}640
                                & 26{,}811    & 26{,}811    & 22{,}108 \\
Adj. $R^2$                      & 0.43        & 0.41       & 0.42
                                & ---         & 0.43        & 0.39 \\
\bottomrule
\end{tabular}

\footnotesize\textit{Notes.} Each column reports the treatment
coefficient from a separate regression. Column (1) is the baseline
two-way fixed-effect specification. Column (2) restricts to non-SOE
firms. Column (3) uses 1:1 nearest-neighbor propensity-score-matched
sample on pre-period size, leverage, ROA, and industry. Column (4) is
the Callaway-Sant'Anna doubly-robust group-time ATT, weighted by
group size. Column (5) reports wild-cluster-bootstrap $p$-values in
brackets (999 replications). Column (6) uses three-year cash ETR as
the outcome. Standard errors in parentheses, clustered at firm and
province-year level except as noted. $^{***} p<0.01$, $^{**} p<0.05$,
$^{*} p<0.10$.
\end{table}
```

## Drafting checklist

- [ ] 800–1,200 words.
- [ ] 3–5 tables (or one multi-panel mega-table).
- [ ] Pre-trends + Rambachan-Roth subsection present.
- [ ] At least one of \citet{sunabraham2021}, \citet{callawaysantanna2021}, \citet{borusyakjaravelspiess2024} cited and implemented.
- [ ] Placebo (fake-date) test present.
- [ ] Wild cluster bootstrap result reported.
- [ ] Each robustness subsection ends with one sentence stating that the result is robust.
- [ ] No new claims about mechanism in Section 6 (mechanism is Section 5.6 or Section 7).
