# 02 — Related Literature

Target length: **800–1,200 words**. Top-journal lit reviews are **short, organized, and contrastive** — they exist to position your contribution, not to demonstrate that you have read the literature.

## Organization: 3 sub-questions, NOT chronological

Top accounting/economics papers organize the literature section around **2–4 thematic sub-questions**, each as a separate sub-section. Chronological (Smith 2001 → Jones 2005 → Lee 2010 → …) is the field-journal default and signals to the editor that you have not yet found your contribution.

For our paper, the three natural sub-questions are:

1. **Corporate tax avoidance — determinants and measurement.** What causes firms to avoid tax, and how do we measure it? (Hanlon-Heitzman survey style.)
2. **Tax-authority enforcement and monitoring.** Does enforcement deter avoidance, and through what channel? (Hoopes-Mescall-Pittman, Hanlon-Hoopes-Shroff.)
3. **Auditor characteristics and tax outcomes.** Do individual signing-auditor traits affect clients' tax positions? (DeFond et al. 2025, Lim et al. 2025, He et al. 2021, Khavis et al. 2025.)

For Design B (Golden Tax Phase IV) the weights shift toward subsections 1 and 2; for Design A (gender-pair switch) toward 1 and 3.

## Within each sub-section: the 3-paragraph pattern

```
Paragraph 1: One-sentence motivating question + 2–4 seminal U.S./international papers.
Paragraph 2: 2–4 most relevant China-specific or directly competing papers.
Paragraph 3: One-sentence statement of what is missing from this body of work
             and how our paper addresses it.
```

## Citation mechanics (natbib)

- `\citet{key}` for **narrative** citations: "\citet{hoopesmescallpittman2012} show that..." → "Hoopes, Mescall, and Pittman (2012) show that..."
- `\citep{key}` for **parenthetical** citations: "Tax monitoring deters avoidance \citep{hoopesmescallpittman2012}." → "...avoidance (Hoopes, Mescall, and Pittman, 2012)."
- Multi-paper parenthetical clusters: `\citep{a2010, b2014, c2021}` → "(A, 2010; B, 2014; C, 2021)."
- Page cites: `\citep[p. 234]{hanlon2010}` → "(Hanlon, 2010, p. 234)."
- Author already named in sentence + parenthetical year: `\citeyearpar{key}` → "(2014)".

Rule of thumb: about 60% of citations in the lit-review section should be `\citet` (you are commenting on what they did) and 40% `\citep` (you are using them as support).

## Opening-sentence patterns (3 per sub-question)

### Sub-question 1 — determinants of tax avoidance

- "A large literature investigates firm-level determinants of corporate tax avoidance (e.g., \citealt{chenchenchengshevlin2010}; \citealt{hanlonheitzman2010})."
- "Cross-sectional variation in effective tax rates is large and persistent, and a substantial fraction remains unexplained by firm fundamentals (\citealt{hanlonheitzman2010})."
- "Whether and how <governance / ownership / managerial trait> shapes tax aggressiveness has been studied extensively (e.g., \citealt{chenchenchengshevlin2010})."

### Sub-question 2 — tax-authority enforcement

- "\citet{hoopesmescallpittman2012} provide foundational evidence that IRS audit probabilities deter U.S. corporate tax avoidance."
- "A second strand examines whether tax-authority monitoring also improves financial-reporting quality \citep{hanlonhoopesshroff2014}."
- "Evidence on tax enforcement in emerging markets is comparatively sparse, despite policy interest in <Golden Tax / e-invoicing / digital platforms>."

### Sub-question 3 — auditor characteristics and tax outcomes

- "Recent work shows that signing-auditor attributes — not only firm-level auditor characteristics — shape clients' tax outcomes (\citealt{defondqisizhang2025}; \citealt{limshevlinwangxu2025})."
- "\citet{heliMonroesi2021} examine signing-auditor diversity in the dual-signature Chinese setting and link it to audit quality."
- "\citet{khavisshenemanszerwo2025} document that female representation in audit teams is associated with higher audit quality in U.S. data."

## The "differentiating contribution" paragraph (mandatory closer)

Every top-journal lit-review section closes with one paragraph that:

1. Re-states the closest **one** prior paper.
2. Identifies **one sharp dimension** on which our paper differs (setting, identification, mechanism, outcome).
3. Avoids any phrase containing "we are the first." Use "we extend" or "we differ in three respects" instead.

## Skeleton

```latex
\section{Related Literature} \label{sec:lit}

\subsection{Corporate Tax Avoidance: Determinants and Measurement}

A large literature investigates firm-level determinants of corporate
tax avoidance \citep{hanlonheitzman2010, chenchenchengshevlin2010,
<<add 2 more>>}. The standard outcomes are the cash effective tax
rate (CETR), the GAAP effective tax rate, and book-tax differences
(BTD) \citep{hanlonheitzman2010}. <<2–3 sentences synthesizing what
this literature has shown about ownership, governance, and
managerial determinants.>>

In the Chinese setting, prior work documents that <<state ownership /
political connection / family ownership>> is associated with
<<higher / lower>> tax aggressiveness \citep{<<china-tax key1>>,
<<china-tax key2>>}. <<1–2 sentences on China-specific institutional
context and what the BTD measure captures in China>>.

What is missing from this body of work is <<one sentence>>. We
contribute by <<one sentence>>.

\subsection{Tax-Authority Enforcement and Monitoring}

\citet{hoopesmescallpittman2012} provide foundational evidence that
IRS audit probabilities deter U.S. corporate tax avoidance, raising
cash effective tax rates by roughly two percentage points as audit
risk moves from the 25th to the 75th percentile.
\citet{hanlonhoopesshroff2014} extend this evidence to financial-
reporting quality, showing that monitoring spillovers improve
reporting beyond the direct deterrence channel.

In emerging markets, <<2–3 sentences citing 2024–2026 China-tax
papers on Golden Tax Phase III; flag that Phase IV is unstudied>>.

What is missing is <<one sentence on the real-time-platform
channel / the post-2022 setting / the magnitude in the world's
largest emerging tax system>>. We address this gap by <<one sentence>>.

\subsection{Auditor Characteristics and Tax Outcomes}

Recent work demonstrates that individual signing-auditor attributes
shape clients' tax positions. \citet{defondqisizhang2025} find that
Chinese firms whose signing auditor is a Certified Tax Agent and
provides no non-audit tax services exhibit lower tax aggressiveness.
\citet{limshevlinwangxu2025} document tax-knowledge diffusion through
shared individual auditors. \citet{heliMonroesi2021} show that
cognitive diversity in the signing-auditor pair is positively
associated with audit quality in China. \citet{khavisshenemanszerwo2025}
find that female representation in U.S. audit teams improves audit
quality and reduces fees.

<<Design A only: 2 sentences linking team-composition results to the
tax-planning setting and explaining why gender composition could
matter for tax outcomes — risk preferences, ethical reasoning,
information processing.>>

What is missing is <<one sentence isolating composition from
expertise / the dynamic effect around a within-firm switch>>. We
extend this literature by <<one sentence>>.

\subsection{Our Contribution}

The paper closest to ours is \citet{defondqisizhang2025}, who study
signing-auditor tax expertise in China. We differ in three respects.
First, <<setting: GTP-IV is an enforcement-side shock, not an
auditor characteristic / gender composition is a team-composition
attribute, not expertise>>. Second, <<identification: staggered DID
with parallel-trend tests / within-firm event-study around a switch,
absorbing all time-invariant firm heterogeneity>>. Third,
<<mechanism: monitoring-technology versus expertise / cognitive-
diversity versus individual-expertise>>. Together these differences
imply <<one sentence on the additional knowledge our paper produces>>.
```

## Drafting checklist

- [ ] 800–1,200 words total.
- [ ] Three subsections, each 2–3 paragraphs.
- [ ] Every citation cross-checked for author + year + journal.
- [ ] Every sub-section closes with one sentence stating "what is missing."
- [ ] Final "Our Contribution" subsection names the closest prior paper explicitly.
- [ ] No "comprehensive review," no "extensive literature," no "we are the first."
- [ ] At least 4 of the cited papers were published in TAR, JAR, JAE, RAST, CAR, JFE, JF, RFS, AER, QJE, JoE, or ReStud.
