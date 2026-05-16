# Phase 3 — Code

You are my econ RA. The paper folder is `<PAPER_FOLDER_PATH>`. Read `design.md`, `goal.md`, and inspect the schema of every file in `data/`. Read `~/Desktop/econ_test/econ-toolbox.md` to pick the right Python package per method, and `~/Desktop/econ_test/style.md` for table/figure conventions.

Write **modular Python** under `code/`, one file per stage. Each file is runnable on its own from `code/`. Save cleaned intermediates to the paper-local `data/processed/`.

**Data paths.** Raw data lives at the repo-root `data/` directory (shared across all papers). The paper folder sits at `project*/papers/<slug>/`, so from `code/` the raw root is three levels up:

```python
from pathlib import Path
PAPER = Path(__file__).parent.parent          # project*/papers/<slug>/
RAW   = PAPER.parent.parent.parent / "data"   # ../../../data/  (repo root /data)
PROC  = PAPER / "data" / "processed"          # paper-local intermediates
```

Never copy raw files into the paper folder. Read from `RAW`, write to `PROC`.

### Files to create

- **`01_clean.py`** — load raw data from `../../../data/<file>`, apply sample filters from `design.md`, construct variables (treatment, outcome, controls), save the analysis dataset to `data/processed/analysis.parquet`. Log filter-by-filter row counts.
- **`02_descriptive.py`** — summary stats, balance, univariate plots. Writes `results/tab_summary.tex`, `results/tab_balance.tex`, `results/fig_*.pdf`.
- **`03_main.py`** — headline regression + 2-3 nested alternatives. Output `results/tab_main.tex`.
- **`04_robustness.py`** — every check from the design's robustness queue. Output `results/tab_robust_*.tex`.
- **`05_heterogeneity.py`** — heterogeneity per `design.md`. Output `results/tab_het.tex` + `results/fig_het.pdf`.

### Conventions

- Use `pyfixest` / `linearmodels` / `statsmodels` / `econml` / `doubleml` per `econ-toolbox.md`. Prefer `pyfixest` for FE.
- **Always cluster SE** at the level in `design.md`. Document in code comments AND table notes.
- LaTeX tables via `Stargazer` or `pyfixest.etable` — booktabs, SE in parens, stars `*** ** *`. Match `style.md`.
- Plots as PDF, grayscale-friendly, ≥10pt fonts.
- Log every run to `results/run_log.txt` (timestamp, file, N, main coef, SE, p).
- All paths via `pathlib.Path(__file__).parent`. Never hardcode absolute paths.

### After running

Write `results/interpretation.md`: one paragraph per spec in plain English.

**STOP for me to inspect tables, plots, and interpretation before drafting.**
