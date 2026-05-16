# Phase 5 — Polish

You are my econ RA. The paper folder is `<PAPER_FOLDER_PATH>`. Final polish on `draft/paper.tex`. Read `~/Desktop/econ_test/style.md` first; every check below maps to a rule in it.

### 1. Bibliography completeness
Grep every `\cite*{...}` key in `draft/paper.tex`. For each key, verify it resolves in `draft/refs.bib`. For missing keys, fetch the BibTeX from Google Scholar / NBER / RePEc and append to `refs.bib`. Report any key you couldn't resolve.

### 2. Table format pass
For every `results/tab_*.tex`:
- `booktabs` rules only (no `\hline`, no vertical bars).
- SE in parentheses below coefficients (not p-values, not t-stats).
- Stars: `*** p<0.01, ** p<0.05, * p<0.1`. Note included.
- FE rows labeled ("Year FE", "Firm FE": Yes/No).
- Cluster-SE level disclosed in the note.
- `N` row present. `R^2` row where meaningful.
- Decimal places: 3 for coef, 3 for SE, 2 for `R^2`.

### 3. Figure format pass
For every `results/fig_*.pdf`:
- Grayscale-friendly (distinguishable B&W).
- Sans-serif, ≥10pt at final size.
- Axis labels are real words, not column names.
- Note + Source line in the LaTeX caption.
- No significance brackets on plots (significance lives in the table).

### 4. Cross-reference check
For every `Table~\ref{...}`, `Figure~\ref{...}`, `Section~\ref{...}` in `paper.tex`, verify the target `\label{...}` exists. Report any dangling refs.

### 5. Word count by section
Report: abstract / intro / lit / data / strategy / results / robustness / conclusion. Flag any section >2x the typical length.

### 6. Acknowledgments + data availability
Add a stub `\section*{Acknowledgments}` and a `\section*{Data availability}` paragraph before the bibliography.

### 7. Compile
Run from `draft/`:
```bash
pdflatex paper.tex
bibtex paper
pdflatex paper.tex
pdflatex paper.tex
```
Report any LaTeX errors or undefined references.

**STOP. Report all findings as a checklist with green / red status per item.**
