# Style

## Writing voice

- Terse. Active verbs. One idea per sentence.
- No hedging: drop "arguably," "it seems," "one might suggest," "interestingly."
- No LLM tells: drop "delve," "navigate," "leverage," "in the realm of," "it is important to note that," "moreover" / "furthermore" as paragraph openers.
- No throat-clearing: do not write "This paper studies X" twice (intro vs abstract). Pick one phrasing, vary the second.
- Cite by author when the name carries weight (`\citet{}`), parenthetical otherwise (`\citep{}`).
- Numbers in prose: spell out one through nine, digits for 10+, always digits with units (3 percentage points, 5 years).
- Effect direction in plain English before the magnitude: "raises wages by 4.2%" not "the coefficient on T is 0.042."

## Journal target

Default: generic working-paper format (12pt, single-column, double-spaced). Flip a flag later for journal house style.

| Journal | Notes |
|---------|-------|
| AER | Single-column, AER bibstyle, supplementary appendix as separate file |
| QJE | Two-column, JEL codes mandatory, structured abstract |
| JPE | Single-column, Chicago bibstyle, no abstract heading word |
| ReStud | Single-column, ReStud bibstyle |
| JoE / JAE | Elsevier class, harvard bibstyle |

## Citation

- Chicago author-date. BibTeX.
- `\citet{smith2020}` → "Smith (2020)" — use when the author is the subject of the sentence.
- `\citep{smith2020}` → "(Smith 2020)" — use parenthetically.
- Multiple cites: `\citep{smith2020,jones2021}`.
- Page numbers: `\citep[p.~45]{smith2020}`.

## Tables

- `booktabs` only (`\toprule`, `\midrule`, `\bottomrule`). No vertical rules.
- SE in parentheses below coefficient, **not** p-values.
- Stars: `*** p<0.01, ** p<0.05, * p<0.1`. Disclose in note.
- FE rows labeled explicitly: "Year FE: Yes", "Unit FE: Yes".
- Cluster level in note: "Standard errors clustered at the firm level."
- Sample-size row (`N`) and `R^2` row (where meaningful) at the bottom.
- 3 decimal places for coefficients, 3 for SE, 2 for R^2.
- Column headers are dependent-variable names, not "(1)(2)(3)" alone.

## Figures

- Grayscale-friendly: distinguishable when printed B&W. Use shape + line style, not color alone.
- Sans-serif Helvetica or Arial, ≥10pt at final size.
- No chartjunk: no 3D, no gradient fills, no drop shadows.
- No significance brackets / asterisks on plots; put significance in the accompanying table.
- Canonical figure types: coefplot, event-study plot, binscatter, RDD plot, map. Pick the one that shows the identification, not just the result.
- Every figure has a note line ("Notes: ...") and source line ("Source: ...") in the caption.

## Notation

- Greek letters where standard: `\beta` for slopes, `\alpha` for intercepts/FE, `\epsilon` for errors, `\tau` for treatment effects.
- Hat for estimates: `\hat{\beta}`.
- Single-letter variables in equations; words in prose.
- Indices: lowercase `i` for unit, `t` for time, `g` for group, `c` for cohort. Subscripts always.
- Treatment indicator: `D_{it}` (binary) or `T_{it}` (intensity). Outcome: `Y_{it}`.
