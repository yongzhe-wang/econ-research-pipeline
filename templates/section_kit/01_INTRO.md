# 01 — Introduction

Target length: **1,400–1,800 words** (5 paragraphs, ~280–360 words each, occasionally a sixth roadmap paragraph). At AER/TAR/JAE this section is *the* selection device: 70% of desk-reject decisions are made by the end of paragraph 3.

## The 5-paragraph intro formula

| Paragraph | Job | Target words |
|-----------|-----|--------------|
| P1 | Problem statement — why this matters in the real world *and* in the literature | ≤200 |
| P2 | What we know vs. the gap — sets up the contribution | ~200 |
| P3 | What we do — data + identification + sample in plain English | ~200 |
| P4 | Headline result + economic magnitude | ~150 |
| P5 | Contributions (3 bullets) + roadmap (1 sentence) | ~150 |

Some papers add a sixth paragraph (P5a) listing robustness/extension highlights before the contributions paragraph. Keep optional.

---

## P1 — Problem statement

**Job.** In ≤200 words: tell the editor (a) what real-world question you are answering, (b) why a thoughtful empirical researcher should care, and (c) why now.

**Structure.**
1. One-sentence stylized fact (numerical if possible).
2. One sentence on the policy / managerial / theoretical tension.
3. One sentence on why the question is open ("yet evidence remains scarce because…").

**Opening-sentence patterns** (three from exemplars; reword for your own paper):

- "Whether <X> deters <Y> is a long-standing question in <field>." (Hoopes, Mescall & Pittman 2012 *TAR*, style)
- "<Phenomenon> has expanded rapidly in <country/period>, raising concerns about <consequence>." (Cengiz, Dube, Lindner & Zipperer 2019 *QJE*, style)
- "<Group> control a disproportionate share of <outcome>, yet little is known about how they <behave>." (Chen, Chen, Cheng & Shevlin 2010 *JFE*, style)

**What NOT to write.**
- "Tax avoidance has been an important topic in accounting research."
- A textbook definition of your dependent variable.
- A two-paragraph history of Chinese corporate taxation. (Move it to Section 2 or an institutional-background subsection.)

---

## P2 — What we know vs. the gap

**Job.** Acknowledge the closest 3–6 prior papers and identify *one* sharp gap that *this paper* fills. This is **not** a literature review (Section 2 is). One paragraph, one gap.

**Structure.**
1. "A growing literature documents <X> (citep cluster of 3–5 papers)."
2. "However, this evidence largely focuses on <Y>; we know little about <Z>." (the gap)
3. Optional: one sentence on why filling the gap is hard (data unavailability, identification challenge, etc.).

**Opening-sentence patterns.**
- "A growing literature examines <X> (\citealt{a}; \citealt{b}; \citealt{c})."
- "Despite extensive evidence on <X>, three features of <our setting> remain unexplored."
- "Existing work on <X> in the U.S. setting <citep> may not generalize to <our setting> because <reason>."

**What NOT to write.**
- A "lit table" listing 20 papers.
- Throwaway citations (one mention, no synthesis).
- Reading-list prose ("Smith (2010) finds A. Jones (2012) finds B. Lee (2015) finds C."). Synthesize, do not list.

---

## P3 — What we do (data + identification)

**Job.** In ≤200 words, give the referee the empirical setup: sample, period, treatment, comparison, and the identifying handle. If P3 does not communicate identification in plain English, the paper will get a referee-1 "I'm not convinced this is causal" letter regardless of how clever the back end is.

**Structure.**
1. One sentence: sample and period. ("We use a panel of <N> A-share firm-years over <2010–2024> from <CSMAR / WIND / CSRC>.")
2. One sentence: the identification strategy. ("We exploit <SHOCK / WITHIN-FIRM SWITCH> as a quasi-experiment …")
3. One sentence: the comparison. ("…comparing treated firms to <control group> before and after <event>.")
4. One sentence: identifying assumption. ("The design rests on parallel pre-trends and the assumption that <SHOCK> timing is uncorrelated with <CONFOUND>.")
5. One sentence: estimators. ("We estimate two-way fixed-effect DID and, for robustness, the Callaway–Sant'Anna and Sun–Abraham estimators.")

**Opening-sentence patterns.**
- "We use <event> as a quasi-natural experiment to identify <object>."
- "Our identification strategy exploits <source of variation>, which is plausibly exogenous to <confound> because <reason>."
- "We estimate the dynamic effect using an event-study specification around <year 0>."

**What NOT to write.**
- The full estimating equation. (Belongs in Section 4.)
- A list of every variable. (Belongs in Section 3.)
- Hedging that pre-empts the result ("We make no causal claims…"). Either you have causal identification or you don't.

---

## P4 — Headline result + economic magnitude

**Job.** State the main finding, its magnitude in interpretable units, and a one-sentence economic interpretation. **Magnitude is mandatory.** A coefficient without a magnitude is a field-journal abstract.

**Structure.**
1. The headline: "We find that <treatment> is associated with a <X percentage-point / Y SD> <increase / decrease> in <outcome>."
2. Magnitude relative to a benchmark: "This corresponds to <Z>% of the sample-mean <outcome> and is statistically significant at the <1>% level."
3. Robustness one-liner: "The result is robust to <alternative estimators / alternative samples / alternative outcomes>."
4. Optional mechanism teaser: "The effect concentrates in <subgroup>, consistent with <channel>."

**Opening-sentence patterns.**
- "We find that …"
- "Our main result is …"
- "Following <treatment>, <outcome> <changes> by …"

**What NOT to write.**
- "The coefficient on <X> is 0.034 (t = 2.71)." (Save for the results section.)
- "Results are mixed." (If they are, the paper is not ready for top-journal submission.)
- Five different magnitudes for five different specifications.

---

## P5 — Contributions + roadmap

**Job.** Three bullet-style contributions (in prose, not bullets) and a one-sentence roadmap. Each contribution is one sentence positioning *this paper* against the closest cluster of prior work.

**Structure.**
1. "We contribute to <literature A> by …"
2. "We extend <literature B> by …"
3. "We complement <directly competing paper, e.g., DeFond et al. 2025 JAE> by …"
4. Optional fourth bullet: a methodological contribution.
5. Roadmap: "Section 2 reviews related literature; Section 3 describes the data; Section 4 presents the empirical strategy; Section 5 reports results; Section 6 discusses robustness; Section 7 concludes."

**Opening-sentence patterns.**
- "Our paper makes three contributions."
- "We contribute to a growing literature on <X>."
- "First, we …"

**What NOT to write.**
- "This is the first paper to …" unless it really is and you can defend it.
- A contribution that is also the contribution of the directly competing paper. Find a sharp differentiator.

---

## Skeleton for our paper

### P1 (use for B *or* A — adapt one fact)

> Tax avoidance reduces <<X% of corporate-income-tax revenue>> in
> <<emerging-market economies>> and remains one of the largest
> unexplained components of cross-firm variation in effective tax rates
> (cite \citealt{hanlonheitzman2010}). <<For B: A central policy
> response is investment in tax-administration technology, but causal
> evidence on whether digital monitoring deters avoidance is scarce
> outside the United States.>> <<For A: A growing literature studies
> auditors' role as tax-planning intermediaries, but the demographic
> composition of the engagement team — distinct from expertise — has
> received almost no attention.>> This paper provides such evidence
> using <<the 2022 nationwide rollout of China's Golden Tax Phase IV
> / within-firm changes in the signing-auditor gender pair>>.

### P2

> A growing literature documents that tax-authority monitoring deters
> avoidance \citep{hoopesmescallpittman2012,hanlonhoopesshroff2014},
> that auditor characteristics shape clients' tax positions
> \citep{defondqisizhang2025}, and that auditor team composition
> matters for audit quality \citep{heliMonroesi2021,
> khavisshenemanszerwo2025}. However, three features remain
> unexplored: <<(i) ... (ii) ... (iii) ...>>. We fill this gap by
> <<one sentence>>.

### P3

> We use <<a panel of N=<<150,000>> A-share firm-years over
> <<2018–2024>>>> from <<CSMAR and CNRDS>>. <<For B: Identification
> exploits the staggered rollout of GTP-IV across <<provinces /
> industries>> beginning in 2022, comparing firms with high vs. low
> pre-period VAT-invoice intensity in an event-study DID design.>>
> <<For A: Identification exploits within-firm switches in the
> signing-auditor gender pair, comparing firm-years before and after
> a switch within the same firm-auditor-office unit.>> Identification
> rests on parallel pre-trends, which we test in event-study plots,
> and on the assumption that <<treatment timing / switch timing>> is
> uncorrelated with unobserved tax-planning shocks. We estimate
> two-way fixed-effect DID and, for robustness, the Callaway-Sant'Anna
> \citep{callawaysantanna2021} and Sun-Abraham
> \citep{sunabraham2021} estimators.

### P4

> We find that <<treatment / mixed-gender pair>> reduces <<book-tax
> differences / cash-ETR avoidance>> by <<X.X percentage points
> (Y SDs of the sample)>>, equivalent to a <<Z%>> change relative to
> the sample mean of <<W>>. The result is robust to alternative
> outcomes, sample restrictions, and heterogeneity-robust estimators,
> and the effect concentrates in <<state-owned / weak-governance /
> low-quality-auditor>> firms.

### P5

> Our paper makes three contributions. First, we provide the first
> event-study evidence on <<real-time digital tax administration in
> the world's largest emerging tax system / signing-auditor gender
> composition in the tax-planning setting>>. Second, we extend
> <<Hoopes, Mescall, and Pittman (2012); Hanlon, Hoopes, and Shroff
> (2014)>> on tax monitoring to <<the digital-platform era / the
> demographic-composition channel>>. Third, we complement
> \citet{defondqisizhang2025} by <<isolating the
> monitoring-technology channel from the auditor-expertise
> channel / isolating composition effects from expertise effects>>.
> The remainder of the paper is organized as follows. Section 2
> reviews related literature; Section 3 describes the data and sample;
> Section 4 presents the empirical strategy; Section 5 reports the
> main results; Section 6 documents robustness; Section 7 concludes.

## Drafting checklist

- [ ] Total length 1,400–1,800 words.
- [ ] P1 does NOT start with "Tax avoidance is an important topic."
- [ ] P2 cites exactly the closest 3–6 papers, not a 20-paper sweep.
- [ ] P3 contains exactly one sentence stating the identifying assumption explicitly.
- [ ] P4 contains exactly one number in percentage points AND one in SDs OR % of mean.
- [ ] P5 contains three contributions, each ≤2 sentences, each naming the literature it updates.
- [ ] No section number is mentioned before the roadmap sentence.
- [ ] No use of "comprehensive," "novel," "very," "extensive," "important."
