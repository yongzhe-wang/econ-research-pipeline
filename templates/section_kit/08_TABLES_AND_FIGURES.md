# 08 — Tables and Figures Conventions

Top-journal tables and figures are not optional aesthetic preferences — they are **referee-screening signals.** A table in MS Word's default formatting, or a Stata-default coefficient plot with cyan-red bars, signals "field journal" in the first 0.5 seconds the editor opens the PDF. Get this right.

## Tables — non-negotiable conventions

1. **LaTeX `booktabs`** is mandatory. Use `\toprule`, `\midrule`, `\bottomrule`. **Never** `\hline`. **Never** vertical bars. **Never** `|c|c|c|`.
2. **Standard errors in parentheses below the coefficient.** Not t-stats, not p-values, not standard errors to the right. The convention is fixed; do not innovate.
3. **Significance stars:** `***` for $p<0.01$, `**` for $p<0.05$, `*` for $p<0.10$. Defined in the table notes.
4. **No vertical lines anywhere.** Horizontal lines only at top, between header and body, and at bottom.
5. **Bottom panel of every regression table** lists, in order:
   - Fixed-effects rows (Firm FE: Yes; Year FE: Yes; etc.)
   - Cluster level
   - $N$
   - Adj. $R^2$ (or pseudo $R^2$ for nonlinear)
6. **Notes paragraph below each table** in `\footnotesize` italics, covering:
   - Sample period and unit of observation.
   - Outcome definition (one sentence) and source.
   - Standard-error clustering level.
   - Significance-star legend.
7. **Numerical formatting:**
   - Coefficients to 3 decimal places.
   - SEs to 3 decimal places, in parentheses, on the line below the coefficient.
   - $N$ written as `26{,}811` (thin-space thousands separator in LaTeX).
   - $R^2$ to 2 decimal places.
8. **Column labels** at the head should be brief specification names — `(1) Baseline`, `(2) + Controls`, `(3) + FE`, `(4) + Ind × Year FE`. Never spec-block-only.

## (a) Coefficient table — full LaTeX template

```latex
\begin{table}[!htbp]
\centering
\caption{Main Specification: Effect of GTP-IV on Cash Effective Tax Rate}
\label{tab:main}
\small
\begin{tabular}{l c c c c}
\toprule
                            & (1)        & (2)        & (3)        & (4) \\
                            & No FE      & + Controls & + Firm \& Year FE & + Ind $\times$ Year FE \\
\midrule
Post $\times$ Treat         & 0.031***   & 0.028***   & 0.024***   & 0.023*** \\
                            & (0.008)    & (0.007)    & (0.006)    & (0.006) \\
\addlinespace
Size$_{t-1}$                &            & $-$0.011** & $-$0.009** & $-$0.008** \\
                            &            & (0.005)    & (0.004)    & (0.004) \\
Lev$_{t-1}$                 &            & 0.018      & 0.014      & 0.012 \\
                            &            & (0.013)    & (0.012)    & (0.011) \\
ROA$_{t-1}$                 &            & $-$0.142***& $-$0.121***& $-$0.119***\\
                            &            & (0.022)    & (0.021)    & (0.020) \\
\addlinespace
\midrule
Firm FE                     & No         & No         & Yes        & Yes \\
Year FE                     & No         & No         & Yes        & Yes \\
Industry $\times$ Year FE   & No         & No         & No         & Yes \\
Cluster                     & Firm/PvY   & Firm/PvY   & Firm/PvY   & Firm/PvY \\
$N$                         & 26{,}811   & 26{,}811   & 26{,}811   & 26{,}811 \\
Adj. $R^2$                  & 0.04       & 0.18       & 0.43       & 0.45 \\
\bottomrule
\end{tabular}

\vspace{0.4em}
\begin{minipage}{0.98\linewidth}
\footnotesize\textit{Notes.} The sample is A-share listed firms,
2018--2024. The dependent variable is the cash effective tax rate
(CETR), defined as cash taxes paid divided by pre-tax income,
winsorized at the 1st and 99th percentiles. Post$_t \times$
Treat$_i$ equals one for firm-years on or after Golden Tax Phase IV
(GTP-IV) adoption in firm $i$'s province. Firm-level controls
follow \citet{chenchenchengshevlin2010} and are defined in
Table~\ref{tab:vardef}. Standard errors in parentheses, two-way
clustered at the firm and province-year level. $^{***}p<0.01$,
$^{**}p<0.05$, $^{*}p<0.10$.
\end{minipage}
\end{table}
```

## Figures — non-negotiable conventions

1. **Grayscale-friendly palette.** Referees print papers; cyan-on-red event-study plots become invisible. Use grayscale or color-blind safe palettes (Wong / Okabe-Ito).
2. **Sans-serif body font** — Helvetica, Arial, or system equivalent, $\geq 10$pt at print size.
3. **Vector format (PDF).** Never PNG. Stata `graph export ".pdf"`; R `ggsave(..., device = "pdf")`; matplotlib `savefig(".pdf")`.
4. **Coefficient plots** use **point + whisker** for 95% CI (not error bars, not shaded bands unless the figure is genuinely continuous in event time).
5. **Event studies:**
   - Vertical line at $\tau = -1$ (the reference / omitted period).
   - Horizontal line at $y = 0$.
   - X-axis label: "Event time (years from treatment)".
   - Y-axis label spells out the units: "Effect on cash ETR (percentage points)".
   - Heterogeneity-robust estimator (CS / SA / BJS) overlaid in lighter shade with offset markers.
6. **Caption** placed below the figure (not in the figure). Notes paragraph immediately below caption in `\footnotesize`.

## (b) Event-study figure caption — full LaTeX template

```latex
\begin{figure}[!htbp]
\centering
\includegraphics[width=0.85\linewidth]{figures/eventstudy_cetr.pdf}
\caption{Dynamic Effect of GTP-IV Adoption on Cash Effective Tax Rate}
\label{fig:eventstudy}

\vspace{0.3em}
\begin{minipage}{0.85\linewidth}
\footnotesize\textit{Notes.} The figure plots event-time
coefficients $\widehat{\beta}_{\tau}$ from
Equation~\eqref{eq:eventstudy}, with $\tau$ measured in years from
GTP-IV adoption in firm $i$'s province and $\tau = -1$ omitted as
the reference. Dots are two-way fixed-effect point estimates;
whiskers are 95\% confidence intervals computed with standard
errors two-way clustered at the firm and province-year level.
Triangles overlay the Callaway-Sant'Anna doubly-robust group-time
ATT \citep{callawaysantanna2021}, aggregated by event time. The
vertical dashed line marks the reference period ($\tau = -1$); the
horizontal solid line marks zero. The sample is A-share listed
firms, 2018--2024.
\end{minipage}
\end{figure}
```

## Common errors to fix before submission

- **Three-decimal coefficients but two-decimal SEs.** Match precision.
- **Stars on coefficients but no legend in the notes.** Always define.
- **`R^2 = 0.0432`.** Round to `0.04`.
- **`N = 26811`.** Use `26{,}811`.
- **Figure PNG at 72 dpi.** Always vector PDF.
- **Coefficient plot with no reference line at zero.** Always add `geom_hline(yintercept = 0)`.
- **Color-coded subgroup lines that are indistinguishable in B&W.** Test by printing to grayscale.
- **Tables wider than `\textwidth`.** Use `\small` or `\footnotesize`, or `\resizebox{\textwidth}{!}{...}`.

## Drafting checklist

- [ ] Every regression table uses `booktabs`.
- [ ] Every table has a notes paragraph in `\footnotesize` italics.
- [ ] Every numerical entry has consistent precision (3 dp for coefs/SEs).
- [ ] Every figure is exported as PDF, not PNG.
- [ ] Every event-study figure has the $\tau = -1$ reference line and a $y=0$ line.
- [ ] Every figure is checked for grayscale readability before submission.
- [ ] Every figure caption references the equation it visualizes.
- [ ] Cluster level disclosed in every regression table.
