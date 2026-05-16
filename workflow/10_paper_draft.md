# Phase 9 — Full Paper Draft

## Objective

First complete LaTeX draft. Every section pulled from `templates/section_kit/`, every citation verified, every prose number traceable to a results file. Compile via Tectonic.

## Inputs

- Locked direction from Phase 5.
- Verified bib from Phase 8.
- Exemplar annotations from Phase 6.
- All results (tables, plots) from Phase 3 / 4.

## Outputs

- `draft/paper.tex` — full LaTeX source.
- `draft/refs.bib` — verified BibTeX file.
- `draft/paper.pdf` — compiled PDF.
- `results/prose_number_audit.csv` — every number in prose, traced to a results file.

## Section order — write in this order

Counter-intuitive: write the intro LAST. The intro summarizes the paper; the paper must exist first.

Order:
1. Data (Section 3) — concrete, easy to write, anchors everything else.
2. Empirical strategy (Section 4) — pulled directly from `design.md`.
3. Results (Section 5) — tables already exist; prose around them.
4. Robustness (Section 6) — checklist-driven from Phase 4 outputs.
5. Mechanism + conclusion (Section 7) — restrained interpretation.
6. Literature review (Section 2) — pulled from `annotated_bib.md` with grouping.
7. Tables and figures (Section 8) — final layout, captions, notes.
8. Intro (Section 1) — last. Five paragraphs, mirrors structure of exemplars from Phase 6.
9. Abstract (top) — second-to-last. Mirrors intro paragraph 1 + 4.

Bib style: Chicago author-date + natbib + booktabs.

## Implementation

### Section 3 — Data

Open `templates/section_kit/03_DATA.md`. Follow its structure:
- Source and coverage paragraph.
- Sample construction subsection (filter log from Phase 0).
- Variables table.
- Summary statistics table.

Every number in the data section must come from `results/data_quality_report.md` or `results/summary_stats.tex`. Do not type numbers from memory.

### Section 4 — Empirical strategy

Open `04_EMPIRICAL_STRATEGY.md`. Paste the estimating equation from `design.md`. Paste the identification threats with mitigations. Cite the methods papers (Callaway-Sant'Anna 2021; Rambachan-Roth 2023; etc.) from verified bib.

### Section 5 — Results

Open `05_RESULTS.md`. For each table:
- One paragraph describing the column structure.
- One paragraph for the headline coefficient: direction in plain English, magnitude, SE in parentheses, statistical significance, fraction of mean / SD as a benchmark.
- One paragraph for what changes across columns.

Example phrasing (from the original project):

```
Table 2 reports the within-firm event-study coefficients on BTD around
FF-loss events. Column 1 includes firm and year fixed effects only;
columns 2-4 progressively add industry-year, province-year, and
firm-level controls. The point estimate on FF-loss is +0.0089
(SE 0.0032, p = 0.006) in the saturated specification of column 4,
representing about 25 percent of the BTD sample mean (0.036). The
coefficient is stable across all four columns within ±0.0008.
```

### Section 6 — Robustness

Open `06_ROBUSTNESS.md`. One subsection per Phase 4 killer test. State the test, the result, the interpretation. Use a single robustness table that stacks all checks (rows = checks, columns = headline coefficient, SE, N).

### Section 7 — Mechanism + Conclusion

Open `07_MECHANISM_AND_CONCLUSION.md`. Mechanism section is restrained: state what was tested, what was significant, what was null. Conclusion is two paragraphs: contribution and limitation.

### Section 2 — Literature review

Open `02_LITERATURE.md`. Group citations by sub-question (not chronology). Each group has 1-2 paragraphs with 3-6 citations. End with a "Contribution" paragraph that states three differentiations from the closest prior work.

### Section 8 — Tables and figures

Open `08_TABLES_AND_FIGURES.md`. Set the layout (booktabs, no vertical rules, 3 decimal places, SE in parens, stars notation). Add notes lines: "Standard errors clustered at firm level in parentheses. *** p<0.01, ** p<0.05, * p<0.1."

### Section 1 — Intro

Open `01_INTRO.md`. Five paragraphs:
1. **Hook + headline.** 3 sentences. End with the locked headline coefficient.
2. **Gap.** What the literature does not answer, with 2 citations.
3. **Data + identification.** One paragraph summary of Section 3 + 4.
4. **Result + falsification.** Headline + binding falsification + HonestDiD.
5. **Contribution.** 3 distinct contributions, named.

### Abstract

150-200 words. Mirrors intro paragraph 1 and 4. Headline coefficient in the abstract. JEL codes if required by journal.

## Prose-number audit

Set up an audit step before compile:

```python
# code/audit_prose_numbers.py
import re
with open("draft/paper.tex") as f:
    tex = f.read()
# extract numbers like 0.0089, 25 percent, etc.
nums = re.findall(r"-?\d+\.\d+", tex)
# write to CSV with line context
# then manually trace each to a results file
```

Every coefficient cited in prose must match a number in `results/`. If a prose number does not match, fix the prose. If a result number does not appear in any table, you have an untraced claim.

## Compile via Tectonic

```bash
cd draft/
tectonic paper.tex
```

First compile is slow (downloads packages). Subsequent compiles are fast.

If compile fails:
- Read the first error message, not the last. Cascading errors are noise.
- Common issues: missing `\bibliography{refs}` line, undefined `\cite` (citation key not in refs.bib), unmatched braces.
- Tectonic does not have `lacheck` built in; run separately if needed.

## What success looks like

- `draft/paper.pdf` compiles clean.
- Every cite key in `paper.tex` has a matching entry in `refs.bib`.
- Every prose coefficient has been audited against a results table.
- The PDF is approximately the right length for the target journal (typically 40-60 pages double-spaced for working paper format).

## What failure looks like

- Compile errors that you patch over without fixing the root. (E.g., commenting out an `\includegraphics` line because the PDF is missing. Generate the missing PDF instead.)
- Prose numbers that "look right" but do not match the actual results. Audit them.
- An intro written before the rest of the paper. The intro will be wrong; you do not yet know what the paper concludes until the other sections exist.
- A lit review that just lists papers. The lit review groups by sub-question and ends with a contribution paragraph.

## Pitfalls

- Drafting from memory rather than from `design.md`. The drift accumulates; by Section 6 the empirical strategy you describe is no longer the one you ran.
- Using verb tenses inconsistently. Pick: "we find X" (present) or "we found X" (past). Stay consistent.
- Citing a coefficient as "0.89" in prose when the table shows "0.0089". Orders-of-magnitude typos are common late at night. Audit.

## Time budget

1-2 weeks for a first full draft. Sections 3-6 are mechanical (1-2 days). Lit review is slow (3-5 days). Intro is the hardest and slowest (2-3 days).
