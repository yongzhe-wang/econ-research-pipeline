# Phase 1 — Literature Trend Scan (Parallel Subagents)

## Objective

Find 20-30 recent papers relevant to your question, identify the gap, propose 3 candidate contribution angles. Output `annotated_bib.md`.

## Inputs

- `goal.md` (RQ, data inventory, hypotheses).
- `data/<slug>_clean.parquet` (from Phase 0).
- `style.md` (voice guide).

## Outputs

- `annotated_bib.md` with 20-30 papers, grouped by sub-question, plus a Gap Analysis section.
- BibTeX entries for every paper, saved inline at the bottom of `annotated_bib.md`.

## Recipe — spawn 5 parallel subagents

The trick is parallelism. Each subagent searches one source family. Master synthesizes after.

### Subagent 1 — NBER + SSRN

Prompt:
```
Search NBER working papers (nber.org/papers) and SSRN economics & finance,
2022-2026, on the topic: <your RQ in one sentence>. Return 8-12 papers
with full citation, finding (one sentence), relevance (one sentence),
ID strategy (DiD/IV/RDD/etc.).
```

### Subagent 2 — Top-5 journals

Prompt:
```
Search recent issues (2023-2026) of AER, QJE, JPE, ReStud, Econometrica
for papers on <RQ>. Return 5-10 papers in the same format.
```

### Subagent 3 — Field journals

For accounting/finance: Journal of Accounting and Economics, JAR, TAR, RAST, JFE, RFS, JF. For labor: JOLE, ILRR. Adjust per topic.

```
Search recent issues (2022-2026) of <field journals> for papers on <RQ>.
Return 5-10 papers in the same format.
```

### Subagent 4 — Chinese-language sources

If the data is Chinese, this is non-negotiable.

```
Search CNKI (cnki.net), Google Scholar with Chinese keywords, and
Chinese econ working paper repositories (e.g., NSD-PKU, CCER) for
papers on <RQ in Chinese>. Return 5-10 papers with citations
transliterated and translated where possible.
```

Without this step, you will write a lit review that misses 30-50% of the relevant Chinese literature on Chinese data.

### Subagent 5 — Citation graph

Pick one seed paper that is clearly closest to your contribution. Use Semantic Scholar Graph API or Google Scholar's "cited by".

```
Starting from <seed paper>, traverse the citation graph: 5 papers
cited by it (its ancestors) and 10 papers citing it (its descendants).
Filter to 2022-2026 for descendants. Return in the same format.
```

## Synthesis (you do this after subagents return)

Combine all five returns. Deduplicate. Group by sub-question, not chronology. Format each entry:

```
### lastname year — short title
**Citation**: full Chicago author-date
**Finding**: one sentence
**Relevance**: one sentence
**ID strategy**: DiD / IV / RDD / SCM / structural / descriptive / theory
```

Add a final section:

```
## Gap analysis

The literature establishes [X], [Y], [Z].

Unresolved tensions:
1. ...
2. ...
3. ...

Candidate contribution angles:

### Angle A: <one-line pitch>
[paragraph: what the angle is, why existing lit can't answer it, what your data + ID lets you say]

### Angle B: <one-line pitch>
...

### Angle C: <one-line pitch>
...
```

## STOP gate

You stop here. The human picks an angle before Phase 2.

## What success looks like

- 20-30 papers, ≥80% from 2020+.
- Every paper has citation, finding, relevance, ID strategy.
- Three candidate angles, each with a clear gap statement and a clear claim about what the data + ID can establish.
- BibTeX entries at the bottom, ready to be copied into `draft/refs.bib`.

## What failure looks like

- 15 papers because the search stopped early. Floor is 20.
- 60% pre-2020 papers because subagents leaned on familiar classics. Fix: re-run Subagents 1 and 2 with explicit "must be 2022 or later" constraint.
- Three "angles" that are all variations of the same angle. Fix: force the angles to differ on at least one of (treatment definition, sample, identification, outcome).
- A bib entry that cannot be verified (no Crossref hit, no SSRN URL, no publisher page). This is fabrication. Delete and replace.

## Pitfalls

- Search snippets as substitutes for reading. A two-line excerpt is not a finding statement. Open the actual paper and read the abstract.
- Citing papers that came up in search but you cannot find a DOI for. They are probably fabricated.
- Skipping the Chinese-language subagent for Chinese data. The Chinese accounting / corporate finance literature has high-quality work that does not appear in English-language search.
- Letting subagents propose angles. Subagents propose papers; the master synthesizes angles. Otherwise you get three independent angles that do not interrelate.

## Citation verification (preview of Phase 8)

Phase 1 produces a draft bib. Phase 8 verifies every entry against a primary source. You can start verification opportunistically during Phase 1 — when a paper looks suspicious, verify immediately rather than letting it ride until Phase 8.

Red flags during Phase 1:
- "Forthcoming" with no volume / no SSRN URL.
- Co-author pairs that are unusual (two famous econometricians from different fields).
- Page ranges with implausible spans (a 4-page AER article).
- Journal name slightly off ("Journal of Accounting and Economics Research" — not a real journal).

## Time budget

Half a day to one day. The parallel subagent run takes 20-30 minutes; synthesis takes 2-4 hours; verification opportunism adds 1-2 hours.
