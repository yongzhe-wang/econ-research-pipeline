# 03 — Data

Target length: **600–900 words** of prose + **two tables** (Variable Definitions, Summary Statistics) + **one Sample-Construction table** (often a panel in Table 1 or a stand-alone Appendix table). The data section is where credibility is built or lost — referees test sample filters first.

## Standard structure

```
3.1 Sample and Data Sources       (≈150 words)
3.2 Sample Construction           (≈150 words + a filtering table)
3.3 Variable Definitions          (≈150 words + a variable-definitions table)
3.4 Summary Statistics            (≈150 words + summary-stats table)
3.5 Balance and Correlations      (≈100 words + a balance table or correlation matrix)
```

Some papers fold 3.5 into Section 4 as a pre-trend / balance check. Either is acceptable.

## 3.1 — Sample and Data Sources

**Job.** One paragraph: where every variable comes from, period covered, frequency, and unit of observation. Be granular: name the database, the field, and the unit.

**Skeleton.**

> Our sample consists of firm-year observations of A-share listed
> companies on the Shanghai and Shenzhen Stock Exchanges from
> <<2018>> through <<2024>>. Financial-statement and ownership data
> are obtained from the China Stock Market and Accounting Research
> (CSMAR) database; auditor and signing-auditor data from CSMAR's
> Audit Files module cross-validated with CNRDS; <<for B: provincial
> Golden Tax Phase IV pilot rollout dates from <<source>>; / for A:
> signing-auditor demographic attributes (gender, age, CPA-license
> year) hand-collected from annual-report cover pages and
> CSMAR auditor profiles>>. <<Macroeconomic and industry-level
> controls from the National Bureau of Statistics of China and
> the China Banking and Insurance Regulatory Commission.>>

## 3.2 — Sample Construction

**Job.** Show the exact filter ladder with counts. The referee should be able to reproduce N from this paragraph alone.

**Skeleton paragraph + table.**

> We start with all A-share firm-years over <<2018–2024>>, yielding
> <<33,452>> firm-years. We drop <<4,108>> observations in the
> financial-services sector (CSRC industry code J), <<621>>
> observations with negative pre-tax income (the cash-ETR
> denominator becomes undefined), <<312>> observations with
> insufficient lagged data, and <<198>> ST/*ST observations.
> <<For B: We further restrict to firms with non-missing pre-period
> VAT-invoice intensity, dropping a further <<1,402>> observations.>>
> <<For A: We require firms to have at least one within-firm
> signing-auditor switch during the sample, leaving <<X>> firms and
> <<Y>> firm-years in the estimation sample.>> The final sample is
> <<N=26,811>> firm-years across <<3,914>> unique firms.

**Sample-construction table (Table 1, Panel A):**

```latex
\begin{table}[ht]
\caption{Sample Construction}
\label{tab:sample}
\centering
\small
\begin{tabular}{lrr}
\toprule
Filter & Dropped & Remaining \\
\midrule
A-share firm-years, 2018--2024                       &        & 33{,}452 \\
\quad Financial-services sector (CSRC industry J)    & 4{,}108 & 29{,}344 \\
\quad Negative pre-tax income                        &   621  & 28{,}723 \\
\quad Insufficient lagged data                       &   312  & 28{,}411 \\
\quad ST / *ST status                                &   198  & 28{,}213 \\
\quad Missing pre-period VAT-invoice intensity (B)   & 1{,}402 & 26{,}811 \\
\midrule
Final estimation sample                              &        & \textbf{26{,}811} \\
\bottomrule
\end{tabular}

\footnotesize\textit{Notes.} Sample of A-share listed firms on the
Shanghai and Shenzhen Stock Exchanges, <<2018--2024>>. Filters applied
sequentially in the order shown. ST/*ST denotes Special Treatment
status assigned by the CSRC.
\end{table}
```

## 3.3 — Variable Definitions

**Job.** Define every variable used in the paper. Defer measurement disputes to a footnote or appendix.

**Skeleton paragraph + table.**

> Our primary outcome is the **cash effective tax rate (CETR)**,
> defined as cash taxes paid divided by pre-tax income, winsorized
> at the 1st and 99th percentiles. The secondary outcome is
> **book-tax differences (BTD)**, defined as <<pre-tax income minus
> taxable income, scaled by lagged assets, winsorized 1/99>>. The
> treatment indicator is <<Post_t × Treat_i for B, or Mixed_{i,t}
> for A>>. Firm-level controls follow \citet{chenchenchengshevlin2010}
> and \citet{defondqisizhang2025}: size, leverage, ROA, R&D
> intensity, PP\&E intensity, intangible intensity, sales growth,
> Big-4 auditor indicator, and SOE indicator.

**Variable-definitions table (Table 1, Panel B):**

```latex
\begin{table}[ht]
\caption{Variable Definitions}
\label{tab:vardef}
\centering
\footnotesize
\begin{tabular}{p{2.3cm} p{8.5cm} p{2.3cm}}
\toprule
Variable & Definition & Source \\
\midrule
\multicolumn{3}{l}{\textit{Panel A: Outcome variables}} \\
CETR$_{i,t}$ & Cash taxes paid divided by pre-tax income; winsorized 1/99. & CSMAR \\
BTD$_{i,t}$  & (Pre-tax income $-$ taxable income) / lag(total assets); winsorized 1/99. & CSMAR \\
\midrule
\multicolumn{3}{l}{\textit{Panel B: Treatment variables}} \\
Post$_t$ $\times$ Treat$_i$ (B) & Indicator equal to one for firm-years on or after GTP-IV adoption in firm $i$'s province; zero otherwise. & MoF rollout list \\
Mixed$_{i,t}$ (A) & Indicator equal to one if signing-auditor pair contains both genders; zero otherwise. & Hand-collected \\
\midrule
\multicolumn{3}{l}{\textit{Panel C: Firm controls}} \\
Size$_{i,t}$        & Natural log of total assets at year-end.                       & CSMAR \\
Lev$_{i,t}$         & Total liabilities / total assets.                              & CSMAR \\
ROA$_{i,t}$         & Net income / total assets.                                    & CSMAR \\
RD$_{i,t}$          & R\&D expenditure / sales.                                     & CSMAR \\
PPE$_{i,t}$         & Net property, plant, and equipment / total assets.            & CSMAR \\
Intang$_{i,t}$      & Intangible assets / total assets.                             & CSMAR \\
SalesGrowth$_{i,t}$ & ($\mathrm{Sales}_t - \mathrm{Sales}_{t-1}$) / $\mathrm{Sales}_{t-1}$. & CSMAR \\
Big4$_{i,t}$        & Indicator equal to one if firm is audited by a Big-4 auditor. & CSMAR \\
SOE$_i$             & Indicator equal to one if ultimate controller is the State.   & CSMAR \\
\bottomrule
\end{tabular}
\end{table}
```

## 3.4 — Summary Statistics

**Job.** Present means, SDs, and selected quantiles. **No comparison test against an out-of-sample benchmark — that goes in Section 4.** Comment in the paragraph only on facts that *matter* for the rest of the paper (skew, mass at zero, censoring).

**Skeleton paragraph + table.**

> Table~\ref{tab:summ} presents summary statistics for the
> estimation sample. The mean cash ETR is <<0.20>>, broadly
> consistent with the statutory <<25\%>> rate net of regional and
> R\&D incentives. <<Right-skew / mass at zero / fraction censored:
> 1 sentence>>. The mean BTD is <<0.02>> of lagged assets;
> <<X>>\% of firm-years exhibit positive BTD. <<For B: <<Y>>\% of
> firm-years are post-GTP-IV adoption; the share of treated firms
> with high pre-period VAT-invoice intensity is <<Z>>\%.>> <<For A:
> <<W>>\% of firm-years feature a mixed-gender signing pair;
> <<V>>\% of firms experience at least one switch during the
> sample.>>

(Table layout — see `08_TABLES_AND_FIGURES.md` for booktabs conventions.)

## 3.5 — Balance and Correlations (optional but recommended)

**Job.** Show that treated and control firms are observably similar **before** treatment on the firm-level controls. This is the single most-asked-for table at first-round revision.

**Skeleton paragraph.**

> Table~\ref{tab:balance} presents pre-period means for treated and
> control firms together with standardized differences. Treated and
> control firms are statistically indistinguishable on all firm-level
> controls except <<Size / Lev>>, for which the standardized
> difference is <<0.X>>. We control for these variables in all
> regressions and confirm in Section~\ref{sec:robustness} that
> entropy-balanced and propensity-score-matched samples yield
> quantitatively similar results.

## Drafting checklist

- [ ] Section 3 prose totals 600–900 words.
- [ ] Sample-construction table reproduces N from raw to final.
- [ ] Variable-definitions table is alphabetized within panel and indexed by `$i,t$` subscript notation.
- [ ] Every variable used anywhere in the paper appears in the variable-definitions table.
- [ ] Summary-stats table reports mean, SD, p25, p50, p75, N for each variable.
- [ ] Winsorization (1/99 or 5/95) disclosed explicitly.
- [ ] Source column populated for every variable.
- [ ] No econometric specification is mentioned before Section 4.
