# Econ Toolbox — Method → Package Map

Python first. R via `rpy2` only when no working Python port exists.

## Core regression

| Method | Python | R fallback | Notes |
|--------|--------|------------|-------|
| OLS | `statsmodels.api.OLS` | `lm()` | Cluster SE via `cov_type='cluster', cov_kwds={'groups': df.cluster_col}` |
| 2SLS / IV | `linearmodels.IV2SLS` | `ivreg::ivreg` | Reports first-stage F, weak-IV diagnostics |
| Fixed-effects panel | `pyfixest.feols` or `linearmodels.PanelOLS` | `fixest::feols` | pyfixest is the Python port of `fixest`; supports multi-way FE, IV, clustering in one call |
| Probit / Logit | `statsmodels.discrete` | `glm()` | Use `Logit` with `cov_type='cluster'`; report marginal effects |
| Tobit | `statsmodels.miscmodels.tobit` (limited) | `AER::tobit` | Python support thin; R via rpy2 if needed |
| Quantile reg | `statsmodels.regression.quantile_regression` | `quantreg::rq` | OK in Python |

## Causal designs

| Method | Python | R fallback | Notes |
|--------|--------|------------|-------|
| DiD (canonical 2x2) | `pyfixest.feols` with interaction | `fixest::feols` | Standard `Y ~ post*treated \| unit + time` |
| Two-way FE DiD | `pyfixest.feols` | `fixest::feols` | Disclose: biased under staggered + heterogeneous effects |
| Staggered DiD (Callaway-Sant'Anna) | `differences` package | `did::att_gt` | R is the reference impl; `did_imputation`-style via Borusyak et al. has a Python port: `did_imputation` (limited) |
| Event study | `pyfixest.feols` with `i(time, treat, ref=-1)` syntax | `fixest::feols` (`i(time, treat)`) | Plot via `pyfixest.iplot` or manual coefplot |
| RDD (sharp/fuzzy) | `rdrobust` (Python port: `rdrobust-py`) | `rdrobust::rdrobust` | Local-linear with bias-corrected CI; bandwidth via `rdbwselect` |
| RD density test | `rddensity` (Python port available) | `rddensity::rddensity` | McCrary 2008 manipulation test |
| Synthetic control | `pysyncon` or `sparsesc` | `Synth::synth` | Original SCM |
| Synthetic DiD (SDiD) | none mature | `synthdid::synthdid_estimate` | Use R via rpy2 |
| Matching | `causalinference.CausalModel`, `dowhy` | `MatchIt::matchit` | Python OK for PSM, NN matching |
| IPW | `causalinference`, `doubleml` | `WeightIt` | OK in Python |

## ML for causal

| Method | Python | Notes |
|--------|--------|-------|
| Double / debiased ML | `doubleml.DoubleMLPLR`, `DoubleMLIRM` | Reference impl; works well |
| Causal forests | `econml.dml.CausalForestDML` or `econml.grf.CausalForest` | Microsoft EconML; mature |
| Generic ML heterogeneity | `econml.metalearners` (X-learner, T-learner, S-learner) | |
| Policy learning | `econml.policy` | |

## Inference

| Method | Python | Notes |
|--------|--------|-------|
| Clustered SE | `linearmodels` (correct by default); `pyfixest` (correct); `statsmodels` (set `cov_type='cluster'` and pass groups) | Always disclose cluster level |
| Two-way clustered SE | `pyfixest` (`vcov={'CRV1': 'fid+yid'}`), `linearmodels` | |
| Wild cluster bootstrap | `wildboottest` (Python) | Use when few clusters (<30) |
| Bootstrap (generic) | `arch.bootstrap`, `scipy.stats.bootstrap` | |
| Randomization inference | `econtools` or hand-rolled | |

## Output

| Need | Python |
|------|--------|
| LaTeX regression table | `Stargazer` (pip: `stargazer`), `pystout`, or `pyfixest.etable` |
| Summary stats table | `pandas.DataFrame.to_latex(... ,booktabs=True)` |
| Coefplot | `pyfixest.iplot` or `matplotlib` |
| Event-study plot | `pyfixest.iplot` |
| Binscatter | `binsreg` (Python port: `binsreg-py`) |

## When R is unavoidable

Use `rpy2` to call R from Python when:
- **`fixest`** — pyfixest covers most cases; fall back for exotic features (multi-step IV, very complex FE).
- **`did`** (Callaway-Sant'Anna) — Python `differences` package exists but `did` is the reference; use rpy2 for replication-grade results.
- **`synthdid`** — no Python port. Mandatory rpy2.
- **`DRDID`** — doubly-robust DiD; R-only.
- **`bacondecomp`** — Goodman-Bacon decomposition; R-only.

Minimal rpy2 pattern:
```python
from rpy2.robjects import r, pandas2ri
pandas2ri.activate()
r('library(did)')
result = r['att_gt'](yname='Y', tname='t', idname='i', gname='g', data=df)
```
