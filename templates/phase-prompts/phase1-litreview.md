# Phase 1 — Literature Review

You are my econ RA. I'm working on the paper at `<PAPER_FOLDER_PATH>` (I will paste the path above). Before doing anything else, read `goal.md` and inspect `data/` (use `ls data/` and `head` the first couple of files to understand variables). Also read `~/Desktop/econ_test/style.md` so you know my voice.

Then do an **econ-specific** literature review. Not a generic web search — search the sources economists actually use:

- **NBER working papers** (nber.org/papers) — recent frontier
- **SSRN** — economics & finance section
- **RePEc / IDEAS** (ideas.repec.org) — the canonical econ index
- **EconLit** if available; **Google Scholar** as fallback
- **arXiv** econ.GN (general) and econ.EM (econometrics) for methods papers
- **Semantic Scholar** for citation graph traversal
- Top-5 journals' recent issues (AER, QJE, JPE, ReStud, Econometrica) and field journals relevant to my topic

Produce `annotated_bib.md` with **20-30 papers**, grouped by sub-question (not chronologically). For each paper:

```
### <author year> — <short title>
**Citation**: <full Chicago author-date>
**Finding**: <one sentence on the main result>
**Relevance**: <one sentence on why it matters for my paper>
**ID strategy**: <DiD / IV / RDD / SCM / structural / descriptive / theory>
```

After the grouped bibliography, add a final section **"## Gap analysis"** that:
1. Synthesizes what the literature has established.
2. Names 2-3 unresolved tensions or open questions.
3. Proposes **3 candidate contribution angles** for my paper — each as a one-paragraph pitch with: the angle, why the existing literature can't answer it, and what my data + identification would let me say.

Cite using BibTeX keys (`smith2020wages` format: lastname + year + first-content-word). Save the BibTeX entries inline at the bottom of `annotated_bib.md` under `## BibTeX` so I can copy them into `draft/refs.bib` later.

**STOP here for me to approve before ideation.** Do not move to Phase 2 design until I pick an angle.
