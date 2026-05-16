# 05 — Results

Target length: **1,200–1,800 words** across **4–6 tables**. The results section is where the paper *demonstrates* the claim made in the introduction. Every table needs a paragraph; every coefficient discussed needs an economic magnitude.

## Order of presentation (mandatory)

```
5.1 Descriptive evidence / event-study figure                 (Figure 1, optional Table 1 cross-tab)
5.2 Main result — pooled DID / pooled switch                  (Table 2 — THE table)
5.3 Economic magnitude paragraph                              (in prose, no new table)
5.4 Dynamic effects — event-study coefficient plot            (Figure 2, optional Table 3)
5.5 Heterogeneity / cross-section                             (Table 4)
5.6 Mechanism evidence                                        (Table 5)
```

Always present results in the order: **descriptive → main → magnitude → dynamics → heterogeneity → mechanism**. Robustness lives in Section 6.

## 5.1 — Descriptive evidence

**Job.** One paragraph that orients the reader before any regression: what does the raw data look like around the treatment?

For Design B: a binned scatter or line plot of CETR by event time, treated vs. control. For Design A: kernel densities of BTD before and after the switch within firm-office cells.

**Skeleton paragraph.**

> Figure~\ref{fig:rawmean} plots the average <<CETR>> for treated
> and never-treated firms by event year. The two series move in
> parallel through <<$\tau = -1$>>, after which the treated series
> rises sharply by approximately <<X percentage points>> within
> two years and stabilizes. This raw pattern motivates the formal
> event-study analysis that follows.

## 5.2 — Main result (Table 2)

**Job.** Present the pooled DID coefficient in 4 columns, increasing the saturation of controls and FE:
- (1) Treatment indicator only.
- (2) + firm controls.
- (3) + firm FE + year FE.
- (4) + firm FE + year FE + industry $\times$ year FE.

Plus one or two additional columns for the second outcome (BTD).

**"Reading the table" paragraph template** — the paragraph that accompanies Table 2:

```text
Table~\ref{tab:main} reports our main results. In column~(1), without
any fixed effects, the coefficient on <Post × Treat> is <coef>
(<SE>), implying that <treated firms in the post period have
<X percentage points> higher CETR than control firms in the pre
period>. Adding firm-level controls (column 2) leaves the
coefficient essentially unchanged. The preferred specification in
column~(3), which includes firm and year fixed effects, yields a
coefficient of <coef> (<SE>), significant at the <1%> level. The
estimate is stable at <coef> (<SE>) when we add industry × year
fixed effects (column 4), absorbing all industry-level common shocks.
The within-firm $R^2$ rises from <0.0X> to <0.0Y> across columns,
indicating that <Post × Treat> explains a non-trivial share of the
remaining within-firm variation in CETR.
```

**Conventions for Table 2.**
- SE in parentheses **below** the coefficient (not t-stats).
- Significance: `***`, `**`, `*` for 1, 5, 10% (in the body of the table).
- Bottom panel of the table:
  - Firm FE: Yes/No
  - Year FE: Yes/No
  - Industry × Year FE: Yes/No
  - Cluster: firm + province-year
  - N
  - Adj. $R^2$
- See `08_TABLES_AND_FIGURES.md` for the LaTeX template.

## 5.3 — Economic magnitude paragraph (CRUCIAL)

This is the paragraph that converts a coefficient into a story. Every top-journal paper has one. Most field-journal papers do not.

**Template.**

```text
The economic magnitude is meaningful. The point estimate in
column~(3) of Table~\ref{tab:main} implies that <treatment> increases
<CETR> by <X.X percentage points> on average. Relative to the
sample-mean <CETR> of <0.20>, this is a <Y%> increase. Equivalently,
the effect is approximately <Z standard deviations> of the within-firm
distribution of <CETR>. Benchmarking against \citet{hoopesmescallpittman2012},
who find that raising IRS audit probability from the 25th to the
75th percentile increases CETR by roughly two percentage points,
our estimated GTP-IV effect is of <similar / comparable / larger>
magnitude. At the sample level, the implied additional tax revenue
collected from <treated firms> is <RMB W billion> over the 2022–2024
window.
```

The four required magnitude conversions:
1. **Percentage points** (raw coefficient).
2. **Percent of mean** (coef / mean of $Y$).
3. **Standard deviations** (coef / SD of $Y$, ideally the within-firm SD).
4. **Benchmark** (relative to a published estimate from the closest prior paper).

## 5.4 — Dynamic effects (Figure 2 / event-study plot)

**Job.** Plot $\beta_{\tau}$ vs. event time, with 95% confidence bands, vertical line at $\tau = -1$ (reference), grayscale-friendly.

**Reading-the-figure paragraph.**

> Figure~\ref{fig:eventstudy} plots dynamic event-study coefficients
> $\hat{\beta}_{\tau}$ from Equation~\eqref{eq:eventstudy} together
> with 95\% confidence intervals. Pre-treatment coefficients
> ($\tau \in \{-3, -2\}$) are statistically indistinguishable from
> zero (joint $F$-test $p = 0.<XX>$), consistent with parallel
> trends. The treatment effect emerges immediately at $\tau = 0$,
> reaches <X.X percentage points> at $\tau = +2$, and stabilizes
> through $\tau = +3$. The point estimates from the
> heterogeneity-robust Callaway-Sant'Anna estimator
> (overlaid in gray) lie within the 95\% confidence bands of the
> two-way fixed-effect estimates, indicating that staggered
> treatment-effect heterogeneity is not driving our results.

## 5.5 — Heterogeneity (Table 4)

**Job.** Show *where* the effect is strongest. Three candidate splits, each in its own panel:

- **Ownership:** SOE vs. non-SOE.
- **Auditor quality:** Big-4 vs. non-Big-4.
- **Pre-period avoidance intensity:** above- vs. below-median pre-period CETR.

Each split gets one paragraph:

```text
Panel A of Table~\ref{tab:het} splits the sample by state
ownership. The treatment effect is <stronger / weaker> for
<non-SOE> firms (coef = <X>, SE = <Y>) than for SOE firms
(coef = <X'>, SE = <Y'>); the difference is significant at the
<5%> level (interaction test reported in row Δ). This pattern is
consistent with <SOEs facing greater political-cost constraints
that already limited their pre-period avoidance / SOEs being more
politically connected and shielded from enforcement>.
```

## 5.6 — Mechanism evidence (Table 5)

**Job.** Test *one* channel that operationalizes the mechanism named in the introduction.

For Design B (GTP-IV): test that the effect operates through reduced VAT input-output gap or reduced related-party transactions.

For Design A (gender-pair switch): test that the effect operates through more engagement hours, higher audit fees, or different opinion modifications.

```text
Table~\ref{tab:mech} reports tests of the mechanism. Column~(1)
shows that <VAT input-output discrepancy / engagement hours>
decreases by <X%> following treatment, consistent with the
<information-asymmetry / cognitive-diversity> channel. Column~(2)
shows no effect on <a placebo outcome the channel should not move,
e.g., total-asset growth>, addressing the concern that the result
reflects a generic post-treatment shift in firm behavior. Column~(3)
formalizes the mediation analysis following \citet{baronkenny1986},
showing that <X%> of the total effect on <CETR> operates through
<channel>.
```

## Skeleton — Section 5 outline

```latex
\section{Results} \label{sec:results}

\subsection{Descriptive Evidence}
Figure~\ref{fig:rawmean} ...

\subsection{Main Specification}
Table~\ref{tab:main} reports our main results ...
<<reading-the-table paragraph>>

\subsection{Economic Magnitude}
<<magnitude paragraph: 4 conversions + 1 benchmark>>

\subsection{Dynamic Effects}
Figure~\ref{fig:eventstudy} plots ...

\subsection{Heterogeneity}
Table~\ref{tab:het} ...

\subsection{Mechanism}
Table~\ref{tab:mech} ...
```

## Drafting checklist

- [ ] 1,200–1,800 words across 4–6 tables.
- [ ] Every table has a paragraph.
- [ ] Every coefficient discussed has all four magnitude conversions (pp, % of mean, SD, benchmark).
- [ ] Main table (Table 2) has 4 columns with increasing FE saturation.
- [ ] Event-study figure has $\tau = -1$ reference line clearly marked.
- [ ] At least one heterogeneity-robust estimator (Callaway-Sant'Anna, Sun-Abraham, or Borusyak-Jaravel-Spiess) is overlaid in the event-study figure.
- [ ] Heterogeneity section has at most 3 splits — not 8.
- [ ] Mechanism section has at most 1 channel, plus a placebo outcome.
- [ ] No robustness tables in Section 5. (Section 6.)
