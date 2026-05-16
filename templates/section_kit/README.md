# Section-by-Section Template Kit

A **drafting scaffold** distilled from top accounting / public-econ journals (TAR, JAR, JAE, RAST, CAR, JFE, JF, RFS, AER, QJE). Use it to draft a paper that *looks and reads* like a top-journal submission from day one — before you have a single result you are proud of.

Target paper: Chinese A-share tax avoidance with one of two identification designs:
- **B — Golden Tax Phase IV (2022) enforcement shock** (DID / event study)
- **A — within-firm signing-auditor gender-pair switch** (event study around the switch)

Every file below is design-agnostic where possible; where it isn't, each file gives **B-version** and **A-version** skeletons.

## How to use this kit

Work the files in order. Do **not** open Overleaf and start writing prose until you have populated every `<<placeholder>>` in files 00–05.

| File | Purpose | When you fill it in |
|------|---------|---------------------|
| `00_ABSTRACT.md` | 200-word abstract skeleton | After you know the headline number |
| `01_INTRO.md`    | 5-paragraph introduction formula | After abstract + before data work |
| `02_LITERATURE.md` | 3-sub-question literature organization | In parallel with intro |
| `03_DATA.md` | Data-section structure + sample-construction table | When you finalize the sample filters |
| `04_EMPIRICAL_STRATEGY.md` | The most important file: identification paragraph, main equation, identifying assumptions | Before running the main regression |
| `05_RESULTS.md` | Order of tables, "reading the table" template, magnitude template | After main results stabilize |
| `06_ROBUSTNESS.md` | Robustness battery + sensitivity analysis | When the main story is fixed |
| `07_MECHANISM_AND_CONCLUSION.md` | Mechanism + 3-paragraph conclusion | Last |
| `08_TABLES_AND_FIGURES.md` | LaTeX `booktabs` table + coefficient-plot conventions | Used throughout |
| `09_BIBTEX_AND_CITATION_STYLE.md` | `natbib` + `refs.bib` conventions | Used throughout |

## Working rules

1. **Do not start with prose. Start with placeholders.** A populated skeleton already 80% determines whether the paper is field-journal or top-journal.
2. **Identification paragraph is sacred.** Spend a week on `04_EMPIRICAL_STRATEGY.md`. Referees decide the paper there.
3. **Magnitude > significance.** Every coefficient discussed in `05_RESULTS.md` must be translated into percentage points, SDs, and a fraction of the baseline mean. No exceptions.
4. **All citations must be real and verified.** Author + year + journal cross-checked. No fabrications.
5. **One headline, one design, one table.** The paper has one main result; Table 2 is its monument. Everything else is supporting.

## What makes a paper "top-journal-ready" vs "field-journal"

**The identification paragraph and the magnitude paragraph.** A top-journal paper convinces the referee in 3 sentences that the variation it exploits is plausibly exogenous, and in another 3 sentences that the magnitude is economically meaningful relative to a plausible benchmark. Everything else is dressing.
