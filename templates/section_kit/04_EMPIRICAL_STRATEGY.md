# 04 — Empirical Strategy

**Target length: 800–1,200 words.** This section is the **single most important** in the paper. Spend a week here. Referee #1 reads this section and writes the first half of the review based on it.

## Section structure

```
4.1 Identification (≈300 words + the identification paragraph)
4.2 Estimating Equation (LaTeX equation + variable rundown)
4.3 Identifying Assumptions (parallel trends / no anticipation / SUTVA)
4.4 Threats and Mitigations (3–5 threats, each addressed)
4.5 Pre-specified Robustness (bulleted list — body in Section 6)
```

## 4.1 — Identification (the most-read paragraph in the paper)

**Job.** In 200–300 words, in plain English, tell the editor:

1. What variation you exploit.
2. Why that variation is plausibly exogenous to the outcome.
3. What the counterfactual comparison is.
4. What survives differencing-out (firm FE, year FE) and why.

### Three exemplar identification paragraphs (verbatim quote ≤30 words each)

**Hoopes, Mescall & Pittman 2012 *TAR*** — IRS audit risk:

> "[We] confronting potential endogeneity with instrumental variables and panel data estimations" — paper uses regional and firm-size variation in IRS audit rates as the identifying handle.

**Cengiz, Dube, Lindner & Zipperer 2019 *QJE*** — minimum-wage event study:

> "We estimate the effect of minimum wages on low-wage jobs using 138 prominent state-level minimum wage changes between 1979 and 2016 in the United States."

**Callaway & Sant'Anna 2021 *JoE*** — staggered DID with covariates:

> "Researchers can flexibly incorporate covariates into the staggered DiD setup with multiple groups and multiple periods." — frames the group-time average treatment effect $ATT(g,t)$ as the unit of identification.

### The identification-paragraph template

```text
Our identification strategy exploits <ONE-SENTENCE SOURCE OF VARIATION>.
<ONE SENTENCE EXPLAINING WHY THIS VARIATION IS PLAUSIBLY EXOGENOUS TO
THE OUTCOME — appeal to a policy timeline, administrative rotation
rule, regulatory shock, etc.>. The empirical comparison is between
<TREATED UNITS> and <CONTROL UNITS> in the <PRE> and <POST> periods,
within the same <FIRM / FIRM-AUDITOR / PROVINCE-INDUSTRY> cell.

By absorbing <FIRM FIXED EFFECTS> we difference out all
time-invariant determinants of <OUTCOME>; by absorbing <YEAR
FIXED EFFECTS> we difference out all common time-series shocks
(e.g., national tax reforms, macroeconomic fluctuations). The
remaining variation identifies the <DYNAMIC / AVERAGE>
treatment effect of <TREATMENT> on <OUTCOME>.

The identifying assumption is <ONE SENTENCE OF PARALLEL TRENDS
LANGUAGE>. We assess this assumption directly in
Section~<RESULTS> by plotting pre-treatment event-time
coefficients and testing their joint nullity.
```

## 4.2 — Estimating Equation

### Design B — staggered DID / event study

```latex
\begin{equation}
Y_{i,t} \;=\; \alpha_i + \lambda_t + \sum_{\tau \neq -1}
\beta_{\tau}\, \mathbb{1}\!\left\{t - G_i = \tau\right\}
\;+\; \mathbf{X}_{i,t}'\,\gamma \;+\; \varepsilon_{i,t},
\label{eq:eventstudy}
\end{equation}
```

where:

- $Y_{i,t}$ is the tax-avoidance outcome (CETR or BTD) for firm $i$ in year $t$.
- $\alpha_i$ is a firm fixed effect; $\lambda_t$ is a year fixed effect.
- $G_i$ is the GTP-IV adoption year in firm $i$'s province (set to $\infty$ for never-treated firms).
- $\beta_{\tau}$ is the event-time coefficient at $\tau$ years from adoption, with $\tau = -1$ omitted as the reference.
- $\mathbf{X}_{i,t}$ is the vector of firm-level controls defined in Section 3.
- $\varepsilon_{i,t}$ is the error term, with standard errors two-way clustered at firm and province-year level (Cameron-Gelbach-Miller 2011).

The pooled DID specification used for the main table:

```latex
\begin{equation}
Y_{i,t} \;=\; \alpha_i + \lambda_t + \beta\, \mathrm{Post}_{i,t}
\;+\; \mathbf{X}_{i,t}'\,\gamma \;+\; \varepsilon_{i,t},
\label{eq:didmain}
\end{equation}
```

where $\mathrm{Post}_{i,t} = \mathbb{1}\{t \ge G_i\}$.

### Design A — within-firm switch event study

```latex
\begin{equation}
Y_{i,t} \;=\; \alpha_{i,o} + \lambda_t + \sum_{\tau \neq -1}
\beta_{\tau}\, \mathbb{1}\!\left\{t - S_i = \tau\right\}
\;+\; \mathbf{X}_{i,t}'\,\gamma \;+\; \varepsilon_{i,t},
\label{eq:switchevent}
\end{equation}
```

where:

- $\alpha_{i,o}$ is a firm $\times$ auditor-office fixed effect (absorbs all time-invariant client-office heterogeneity, isolating within-engagement variation).
- $S_i$ is the year of the signing-auditor gender-pair switch.
- $\beta_{\tau}$ traces the dynamic effect from $\tau = -3$ to $\tau = +3$.
- Standard errors two-way clustered at firm and auditor-office level.

## 4.3 — Identifying Assumptions

State each assumption explicitly. One paragraph per assumption.

1. **Parallel trends.** Absent treatment, treated and control firms' outcomes would have evolved on the same trajectory. Tested by examining pre-treatment $\beta_{\tau}$ for $\tau < -1$; joint $F$-test reported in Section~5.
2. **No anticipation.** Firms did not change behavior in anticipation of treatment. <<For B: GTP-IV rollout dates were announced shortly before pilot launch; we examine $\beta_{-1}$ and $\beta_{-2}$ for anticipatory movement.>> <<For A: Auditor-rotation timing is set by the audit firm under CSRC five-year mandatory rotation; the specific *gender* of the incoming partner is plausibly orthogonal to client tax preferences.>>
3. **SUTVA / no spillovers.** Treatment of one firm does not affect outcomes of another. <<For B: GTP-IV is a within-province enforcement platform — we cluster at province-year and conduct placebo tests on geographically distant control regions.>> <<For A: Independence across signing-auditor switches in different firm-office cells is plausible after firm-office FE.>>
4. **Treatment-effect heterogeneity robust.** Two-way fixed-effect estimators can be biased under heterogeneous effects \citep{sunabraham2021}. We confirm robustness using Callaway-Sant'Anna \citep{callawaysantanna2021} and Borusyak-Jaravel-Spiess \citep{borusyakjaravelspiess2024} imputation estimators.

## 4.4 — Threats and Mitigations

Walk through 3–5 threats. Each gets one paragraph: **threat → why it would bias → mitigation in our design**.

Typical threats for our paper:

| Threat | Why it would bias | Mitigation |
|--------|------------------|------------|
| Confounding policy: 2022 VAT rebate program | Could reduce CETR independently of GTP-IV | Control for province-year FE; exclude firms in VAT-rebate-eligible sectors as placebo |
| Selection into treatment: provinces with weak compliance launch GTP-IV earlier | Reverse causality | Event-study placebo on never-treated provinces; test pre-trends |
| Auditor self-selection (Design A): firms with declining tax aggressiveness might *request* a mixed-gender pair | Endogenous treatment | Restrict to mandatory-rotation switches; firm $\times$ office FE absorbs static client preferences |
| Measurement: cash ETR is volatile for small or loss-making firms | Attenuation bias | Drop loss firms; winsorize 1/99; show consistent results for BTD |
| Differential covariate trends (Lev, ROA differ across treated/control) | Differential pre-trend | Entropy balancing on pre-period firm controls; propensity-score matched sample |

## 4.5 — Pre-specified Robustness Checks

> The following robustness checks are pre-specified before estimation
> and reported in Section~\ref{sec:robust}: (i) alternative outcomes
> (GAAP ETR; long-run cash ETR over 3- and 5-year windows); (ii)
> alternative samples (entropy-balanced, propensity-score-matched,
> non-SOE only, single-Big-4 only); (iii) alternative fixed-effects
> structures (firm + year, firm + industry-year, firm-office + year);
> (iv) alternative clustering (firm only, province only, two-way
> firm-province); (v) Callaway-Sant'Anna and Sun-Abraham
> heterogeneity-robust estimators; (vi) sensitivity to violations of
> parallel trends following \citet{rambachanroth2023}.

## Skeleton — full section

### Design B

```latex
\section{Empirical Strategy} \label{sec:strategy}

\subsection{Identification}
Our identification strategy exploits the staggered rollout of China's
Golden Tax Phase IV (GTP-IV) across provinces beginning in 2022.
GTP-IV is a real-time, big-data invoicing-and-monitoring platform...
<<paragraph following the template above>>

\subsection{Estimating Equation}
Our main event-study specification is
\begin{equation}
... <<Equation \ref{eq:eventstudy}>>
\end{equation}
where ...

The pooled difference-in-differences specification reported in
Table~\ref{tab:main} is
\begin{equation}
... <<Equation \ref{eq:didmain}>>
\end{equation}

\subsection{Identifying Assumptions}
... <<four assumptions>>

\subsection{Threats to Identification}
... <<5 threats, one paragraph each>>

\subsection{Pre-specified Robustness}
... <<robustness list>>
```

### Design A

Identical structure; substitute the within-firm switch language, firm-office FE, and Equation~\eqref{eq:switchevent}.

## Drafting checklist

- [ ] 800–1,200 words.
- [ ] Identification paragraph stands alone and answers (variation / exogeneity / counterfactual / FE absorbed).
- [ ] Main equation is numbered, every symbol defined immediately below.
- [ ] Clustering level stated explicitly.
- [ ] Parallel-trends assumption named "parallel trends" — not "common trends," not "identical trends."
- [ ] At least one paragraph names \citet{sunabraham2021}, \citet{callawaysantanna2021}, or \citet{borusyakjaravelspiess2024} as a robustness check.
- [ ] At least 3 threats named and addressed.
- [ ] No discussion of *results* in Section 4. (Results are Section 5.)
