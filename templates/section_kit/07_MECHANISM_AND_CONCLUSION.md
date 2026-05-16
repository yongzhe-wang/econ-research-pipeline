# 07 — Mechanism and Conclusion

Two short sections at the end of the paper. Both are deceptively important: mechanism converts a finding into a *story*, and conclusion is the only part of the paper most readers will read in full.

## Part A — Mechanism (≈700 words, 1 dedicated table)

If mechanism evidence is light enough, it can live as the last subsection of Section 5 (a la "5.6 Mechanism evidence" in `05_RESULTS.md`). If you have **multiple channels** to test or **mediation analysis** to present, promote it to its own Section 6/7.

### Channel-decomposition convention

Tax-research mechanism sections usually take one of three forms:

| Form | Used when | Example |
|------|-----------|---------|
| **Sub-sample interaction** | You believe channel X amplifies the effect | "Effect is twice as large in firms with low pre-period audit fees." |
| **Intermediate-outcome test** | You can measure the channel directly | "Treated firms increase VAT-invoice-reporting frequency by X%." |
| **Mediation analysis** | You have a clean mediator | Baron-Kenny or modern (Imai-Keele-Tingley) decomposition. |

For our paper, the strongest mechanism evidence is intermediate-outcome:

**Design B:** Post-GTP-IV, do firms exhibit lower VAT input-output gap? Lower related-party-transaction volume? Higher tax-payment timeliness?

**Design A:** Post-switch to a mixed-gender pair, do engagement hours rise? Audit fees rise? Modified-opinion frequency rise? Tax-related notes in 10-K-equivalent disclosures lengthen?

### Mechanism section skeleton

```latex
\section{Mechanism} \label{sec:mechanism}

\subsection{Conceptual Framework}
We hypothesize that <treatment> reduces <tax avoidance> through
<one specific channel — information asymmetry / monitoring intensity
/ cognitive diversity>. Under this hypothesis, the treatment should:
(a) move <intermediate outcome 1> in <direction>;
(b) move <intermediate outcome 2> in <direction>;
(c) not move <placebo outcome>.

\subsection{Intermediate-Outcome Evidence}
Table~\ref{tab:mech} reports estimates of
Equation~\eqref{eq:didmain} with three alternative dependent
variables corresponding to the channel above. <one paragraph per
column>.

\subsection{Sub-sample Heterogeneity Consistent with the Channel}
If <channel> is the operating mechanism, the treatment effect should
be larger in firms where <pre-condition that activates the channel>
is more pronounced. Table~\ref{tab:mech-het} confirms that the
treatment coefficient is <X percentage points> in firms with
<above-median condition> and <Y percentage points> in firms with
<below-median condition>; the difference is significant at the
<5%> level.

\subsection{Alternative Channels}
We consider three alternative explanations: <(1) ... (2) ... (3) ...>.
For each, we examine an intermediate outcome that would respond
under the alternative channel but not under our preferred channel.
The data are not consistent with any of the three alternatives, as
reported in Appendix Table~\ref{tab:alt-channels}.
```

### Mechanism-table layout

```latex
\begin{table}
\caption{Mechanism Tests}
\label{tab:mech}
\centering
\footnotesize
\begin{tabular}{lcccc}
\toprule
                          & (1) VAT-gap & (2) RPT volume & (3) Tax timeliness
                          & (4) Placebo: Asset growth \\
\midrule
Post $\times$ Treat       & $-$0.018*** &  $-$0.024***   & 0.011**
                          & 0.002 \\
                          & (0.005)     &  (0.008)       & (0.005)
                          & (0.011) \\
\addlinespace
Firm FE, Year FE          & Yes & Yes & Yes & Yes \\
Controls                  & Yes & Yes & Yes & Yes \\
$N$                       & 26{,}811 & 26{,}811 & 26{,}811 & 26{,}811 \\
Adj. $R^2$                & 0.51     & 0.38     & 0.42     & 0.29 \\
\bottomrule
\end{tabular}

\footnotesize\textit{Notes.} Each column reports a separate
regression of an intermediate outcome on the GTP-IV treatment.
Columns (1)--(3) are the proposed channel; column (4) is a placebo
outcome that should not move if the channel is correctly specified.
Standard errors in parentheses, two-way clustered at firm and
province-year level. $^{***}p<0.01$, $^{**}p<0.05$, $^{*}p<0.10$.
\end{table}
```

## Part B — Conclusion (≈400 words, 2–3 paragraphs)

### The three-paragraph conclusion formula

**Paragraph 1 — Summary** (≈120 words).
Restate the question, the design, the headline result with magnitude. **Use past tense.** No new evidence.

**Paragraph 2 — Contribution and implications** (≈150 words).
Re-state the three contributions from the introduction in past tense ("we showed…"). One sentence on practical/policy implications. One sentence on what the result *means* substantively (not technically).

**Paragraph 3 — Limitations and future research** (≈130 words).
Two genuine limitations (not throwaway "more research is needed"). Two specific future-research directions tied to the limitations.

### Conclusion skeleton

```latex
\section{Conclusion} \label{sec:conclusion}

We examined <whether GTP-IV deters corporate tax avoidance / whether
within-firm switches in the signing-auditor gender pair affect tax
aggressiveness>. Using <a panel of <N> A-share firm-years over
<2018--2024>>, we showed that <treated firms experienced a
<X percentage-point> increase in cash effective tax rates following
<treatment>, equivalent to a <Y%> increase relative to the
sample-mean cash ETR>. The result is robust to alternative
outcomes, samples, fixed-effects structures, and
heterogeneity-robust estimators, and survives sensitivity analysis
to deviations from parallel trends.

The paper makes three contributions. First, we provided the first
event-study evidence on <real-time digital tax administration in the
world's largest emerging tax system / signing-auditor team
composition in the tax-planning setting>. Second, we extended
\citet{hoopesmescallpittman2012} and \citet{hanlonhoopesshroff2014}
on tax monitoring to <the digital-platform era / the
demographic-composition channel>. Third, we complemented
\citet{defondqisizhang2025} by <isolating the monitoring-technology
channel from auditor expertise / isolating composition from
individual expertise>. For policy, the findings imply that
<digital-platform investment can be a cost-effective enforcement
substitute for additional auditor headcount / engagement-team
composition is a distinct lever from auditor expertise>.

The analysis has limitations. First, <our setting does not allow us
to separate the deterrence channel from the detection channel of
GTP-IV — both operate through the same platform / our gender measure
captures the binary composition of the signing pair and is silent on
the more granular dimensions of cognitive diversity studied in
\citet{heliMonroesi2021}>. Second, <our 2024 sample window precludes
analysis of medium-run effects on real activity / the within-firm
switch design cannot identify the cumulative effect of repeated
exposure to mixed-gender engagement teams over the audit-firm
tenure>. We leave these questions to future work.
```

## Drafting checklist — Mechanism

- [ ] 600–900 words.
- [ ] One mechanism table with at least one placebo outcome column.
- [ ] At least one alternative channel considered and ruled out (in body or appendix).
- [ ] Sub-sample-heterogeneity check consistent with the channel.

## Drafting checklist — Conclusion

- [ ] 350–500 words, exactly 3 paragraphs.
- [ ] Paragraph 1 contains exactly one magnitude number.
- [ ] Paragraph 2 names three contributions in past tense.
- [ ] Paragraph 3 names two specific (not generic) limitations.
- [ ] No new tables or new evidence in the conclusion.
- [ ] No phrase containing "we hope," "we believe," or "more research is needed."
