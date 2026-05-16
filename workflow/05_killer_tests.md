# Phase 4 — Killer Tests

## Objective

Try to kill the result. Whatever survives is the paper. Run the six tests below in order. Document every kill.

## Inputs

- Candidate headline spec from Phase 3.
- `data/<slug>_clean.parquet`.

## Outputs

- `code/02_killers.py` — all six tests in one script.
- `results/placebo_table.tex` — placebo-quartile + placebo-outcome results.
- `results/transition_matrix.tex` — for categorical treatments.
- `results/switch_decomposition.tex` — by switch type / arm.
- `results/honestdid.tex` and `results/honestdid_plot.pdf` — Rambachan-Roth sensitivity.
- `results/withinfirm_eventstudy.pdf` — event-study plot.
- `results/bundled_controls.tex` — bundled-treatment robustness.

## The six tests

### Test 1 — Placebo-quartile (mean reversion check)

Pre-treatment outcome quartile. Re-estimate the treatment effect within each quartile.

```python
# Pre-treatment Y for each treated unit
pre_Y = df.loc[df["treated"] == 1].groupby("unit_id")["Y"].mean()
df["pre_Y_quartile"] = pd.qcut(df["unit_id"].map(pre_Y), 4, labels=[1,2,3,4])

for q in [1, 2, 3, 4]:
    sub = df[df["pre_Y_quartile"] == q]
    m = pf.feols("Y ~ T | unit_id + year", data=sub, vcov={"CRV1": "unit_id"})
    print(q, m.coef()["T"], m.se()["T"])
```

**Kill condition.** If top quartile and bottom quartile show symmetric reversal (top declines, bottom rises) and middle quartiles are flat, the headline is mean reversion, not a treatment effect. Stop and reframe.

**Real failure example.** The 2019 VAT DID in the original project. Top BTD quartile reverted down; bottom reverted up; the headline was an artifact.

### Test 2 — Transition matrix (categorical treatments)

For categorical treatments (e.g., auditor pair gender from FF -> non-FF), build the full transition matrix.

```python
tm = pd.crosstab(df["prior_state"], df["current_state"])
print(tm)
```

**Kill condition.** If 90% of "switches" are one degenerate transition (e.g., FF -> FM dominates and FF -> MM is rare), the treatment is really FF -> FM specifically. Decompose by exact transition cell.

### Test 3 — Asymmetric framing

If the headline is "X-loss raises Y", estimate "X-gain lowers Y" with the same specification.

```python
# loss arm
m_loss = pf.feols("Y ~ FFLoss | firm + year", data=df)

# gain arm
m_gain = pf.feols("Y ~ FFGain | firm + year", data=df)
```

**Kill condition.** If the gain arm produces a symmetric coefficient of opposite sign, the effect is mechanical reversion / regression to the mean.

**Success condition.** If the gain arm is null and the loss arm is significant, you have an asymmetric channel — a real directional story.

**Real success example.** The original project: FF-loss beta = +0.0089, p = 0.006; FF-gain beta = +0.0012, p = 0.61. The asymmetry was the binding falsification.

### Test 4 — HonestDiD (Rambachan-Roth 2023)

Parallel-trends sensitivity. The canonical reference is Rambachan and Roth (2023, ReStud).

Implementation: R via subprocess.

`code/04_honestdid.R`:
```r
library(HonestDiD)
library(fixest)

# Read CSV of event-study coefficients
es <- read.csv("results/event_study_coefs.csv")
# Columns: tau (relative time), coef, se, vcov_path

# Construct vcov matrix from CSV
vcov <- as.matrix(read.csv("results/event_study_vcov.csv", row.names=1))

# Run sensitivity at M-bar grid
out <- createSensitivityResults(
  betahat = es$coef,
  sigma = vcov,
  numPrePeriods = 4,
  numPostPeriods = 4,
  Mbarvec = c(0, 0.5, 1, 1.5, 2)
)

write.csv(out, "results/honestdid.csv", row.names=FALSE)
```

Call from Python:
```python
import subprocess
subprocess.run(["Rscript", "code/04_honestdid.R"], check=True)
honest = pd.read_csv("results/honestdid.csv")
```

**Kill condition.** If the 95% CI crosses zero at M-bar = 0, parallel trends fails even allowing zero violation. The subsample is not causal.

**Real failure example.** The PRC mandatory partner rotation subsample in the original project. Looked like the cleanest slice; failed HonestDiD at M-bar ≈ 0.002. Demoted from headline to corroboration.

### Test 5 — Within-firm event study

If the cross-section gives a result, also run the within-firm version.

```python
# Within-firm: only include firms with both treated and untreated periods
firms_with_variation = df.groupby("firm")["T"].nunique()
firms_with_variation = firms_with_variation[firms_with_variation > 1].index
within = df[df["firm"].isin(firms_with_variation)]

m = pf.feols("Y ~ i(rel_time, treat, ref=-1) | firm + year", data=within)
```

**Kill condition.** Cross-section positive, within-firm null. The cross-section is selection (treated firms differ on Y for reasons other than treatment).

### Test 6 — Bundled-treatment controls

Add contemporaneous controls in {t-1, t, t+1} for any event that might co-occur with the treatment.

For an auditor-change study: CFO change, audit-firm change, audit-fee shock, Big-4 status change.

```python
controls = ["cfo_change_lag1", "cfo_change", "cfo_change_lead1",
            "big4_change_lag1", "big4_change", "big4_change_lead1",
            "fee_shock_lag1", "fee_shock", "fee_shock_lead1"]

formula = f"Y ~ T + {' + '.join(controls)} | firm + year"
m = pf.feols(formula, data=df, vcov={"CRV1": "firm"})
```

**Kill condition.** If the headline coefficient halves once bundled controls are added, the treatment is partly capturing those co-occurring events. The honest report includes the bundled-controls coefficient as the headline.

## Sequencing

Run all six in one batch. Then triage:
- Test 1 fails -> mean reversion. Stop. Reframe.
- Test 2 reveals degenerate transition -> redefine treatment by exact transition cell.
- Test 3 gain arm symmetric -> reframe as mechanical, not directional.
- Test 4 fails at M-bar=0 -> demote to corroboration.
- Test 5 within-firm null -> the paper is about selection or it is no paper.
- Test 6 coefficient halves -> headline is bundled-controls coefficient.

## What success looks like

- All six tests run cleanly.
- The headline survives at least 4 of 6.
- For each test that did not pass cleanly, you have an honest write-up of what the test showed and how it constrains interpretation.

## What failure looks like

- "Test 4 said M-bar = 0.001 breaks significance, but we can argue parallel trends are fine, so the test is wrong." No. Report the result. Demote to corroboration.
- "The placebo failed but we have other reasons to believe the result." Report the placebo. Let the reader decide.
- Cherry-picking subsamples until one survives all six. You can run six subsamples and report the one that survives only if you also report the five that did not, with N and reasons.

## Pitfalls

- Treating HonestDiD's default M-bar=1 as the only number that matters. Report the full curve and the M-bar at which significance breaks.
- Running placebo on the wrong dimension. Placebo by pre-treatment quartile catches mean reversion; placebo by random treatment catches generic shock; you need BOTH.
- Skipping the asymmetric framing because the loss arm is significant. The gain arm IS the test; you cannot skip it.
- Reframing the headline after Phase 4 without updating `goal.md` and `design.md`. Phase 5 is exactly for this; do not skip.

## Time budget

3-5 days. HonestDiD setup takes time the first run (R install, vcov export, sanity check). Subsequent runs are quick.
