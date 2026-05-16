# Phase 4 — Draft

You are my econ RA. The paper folder is `<PAPER_FOLDER_PATH>`. Read, in order:

1. `goal.md`
2. `annotated_bib.md`
3. `design.md`
4. `results/interpretation.md`
5. Every `results/tab_*.tex` and `results/fig_*.pdf` (use `ls results/` and `cat` the .tex files)
6. `~/Desktop/econ_test/style.md` — match my voice exactly: terse, active, no hedging, no LLM tells

Then draft `draft/paper.tex` with sections in econ order:

### Abstract (200 words)
Lead with the headline number. Structure: one-sentence question, one-sentence data + identification, two-sentence main result, one-sentence contribution.

### Introduction (5 paragraphs, the standard econ formula)
1. **Problem statement** — the substantive question; cite the policy/empirical importance.
2. **Why it matters** — the stakes for theory, policy, or a literature.
3. **What I do** — the data, the identifying variation, the method (one sentence each).
4. **Main finding** — the headline number with units and interpretation; brief mention of robustness.
5. **Contribution & roadmap** — three bullets on what's new vs prior literature, then one sentence per remaining section.

### Literature
Organize by **sub-question**, not chronologically. Two-three paragraphs, each anchored on one of the gap-analysis tensions from `annotated_bib.md`. Cite with `\citet{}` when the author is the subject, `\citep{}` otherwise.

### Data
Source, sample period, unit of observation, sample-construction filters (point to `design.md` for full detail), key variables with construction notes, summary stats table via `\input{../results/tab_summary.tex}`.

### Empirical strategy
The estimating equation (LaTeX block), identification argument, threats and mitigations (compressed from `design.md`).

### Results
- Main result: `\input{../results/tab_main.tex}` with 2-3 paragraphs interpreting the coefficient in plain English (drawn from `results/interpretation.md`).
- Heterogeneity: `\input{../results/tab_het.tex}` and `\includegraphics{../results/fig_het.pdf}`.

### Robustness
Walk through each robustness table in 1-2 sentences. `\input{../results/tab_robust_*.tex}`.

### Conclusion
3 paragraphs: recap finding, situate against literature, name 1-2 honest limitations + future directions.

Use `\citet{key}` / `\citep{key}` throughout, keys matching `draft/refs.bib`. Pull missing BibTeX from the `## BibTeX` section of `annotated_bib.md`.

**STOP for me to edit.**
