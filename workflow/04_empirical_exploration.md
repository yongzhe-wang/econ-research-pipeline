# Phase 3 — Empirical Exploration (60+ Regression Battery)

## Objective

Map the empirical landscape before committing to a headline spec. Run a grid of 16-81 regressions varying outcome, sample, FE structure, and controls. Identify the spec whose coefficient and SE are stable across the largest portion of the grid.

## Inputs

- `data/<slug>_clean.parquet`.
- Locked angle from Phase 1 STOP gate (and any updates from Phase 2 red-team).

## Outputs

- `code/01_explore.py` — the regression battery script.
- `results/exploration_log.md` — every spec, every coefficient, every SE.
- `results/exploration_grid.csv` — machine-readable grid.

## Recipe — the grid

Cross all combinations of:

1. **Outcomes** (2-3): primary + 1-2 alternatives.
   Example for BTD analysis: BTD, CETR, AbBTD.
2. **Samples** (2-3): full + 1-2 restricted.
   Example: full panel, non-SOE only, exclude COVID years (2020-2022).
3. **FE structures** (2-3): minimal + 1-2 saturated.
   Example: firm + year; firm + industry-year; firm + industry-year + province-year.
4. **Control sets** (2-3): no controls (FE-only), minimal, full.

That is 2x2x2x2 = 16 minimum; 3x3x3x3 = 81 maximum. Run all of them.

## Implementation pattern

```python
import pyfixest as pf
import pandas as pd
from itertools import product

outcomes = ["BTD", "CETR_w", "AbBTD"]
samples = {
    "full": df,
    "non_soe": df[df["soe"] == 0],
    "ex_covid": df[~df["year"].isin([2020, 2021, 2022])],
}
fe_specs = {
    "firm_year": "| firm_id + year",
    "firm_indyear": "| firm_id + industry^year",
    "firm_indyear_provyear": "| firm_id + industry^year + province^year",
}
controls = {
    "none": "",
    "minimal": "+ size + leverage + roa",
    "full": "+ size + leverage + roa + age_in_panel + tobins_q",
}

rows = []
for y, (sname, sdf), (fname, fe), (cname, ctrl) in product(
    outcomes, samples.items(), fe_specs.items(), controls.items()
):
    formula = f"{y} ~ FFLoss {ctrl} {fe}"
    try:
        m = pf.feols(formula, data=sdf, vcov={"CRV1": "firm_id"})
        b = m.coef()["FFLoss"]
        se = m.se()["FFLoss"]
        p = m.pvalue()["FFLoss"]
        n = m._N
        r2 = m._r2
    except Exception as e:
        b, se, p, n, r2 = None, None, None, None, str(e)
    rows.append(dict(outcome=y, sample=sname, fe=fname, controls=cname,
                     beta=b, se=se, p=p, n=n, r2=r2))

grid = pd.DataFrame(rows)
grid.to_csv("results/exploration_grid.csv", index=False)
```

## Read the grid

After the grid runs, ask three questions:

1. **Stability.** For the main outcome, how many spec combinations produce a coefficient within ±20% of the median coefficient? Stability share = that count / total specs. If <50%, the result is fragile.

2. **Sign agreement.** Across all specs, what fraction have the same sign? <90% is a flag.

3. **SE behavior.** Does the SE balloon under any specific FE structure? If province-year FE doubles the SE, that FE is absorbing treatment variation — be cautious about including it in the headline.

The candidate headline is the spec whose coefficient and SE are closest to the grid median, with a stable sign across most cells.

## Document everything in `results/exploration_log.md`

```markdown
# Exploration log — <slug>

## Grid summary

- Specs run: <N>
- Specs converged: <N-failed>
- Failed specs and reason: ...

## Stability

- Main outcome <Y>: median beta = X, within ±20% of median in <K> of <N> specs.
- Sign agreement: <P>% positive, <Q>% negative, <R>% zero.

## Candidate headline

Spec: outcome=Y, sample=full, FE=firm+industry-year+province-year, controls=full.
- beta = X, SE = Y, p = Z, N = K.
- Stability: this coefficient is within ±20% of the grid median in <K>/<N> nearby specs.

## Knife-edge results to investigate

- Spec X has |beta| = Y, far from median. Why?
- Spec Z has p-value 0.99. Why?
```

This log is the audit trail. If someone later asks "did you spec-mine?", the log answers: here are 60 specs I ran; the headline is the one that was robust across them. Without the log, the question is unanswerable.

## What success looks like

- Grid CSV with one row per spec.
- Stability share ≥50% for the main outcome.
- Sign agreement ≥90% across specs.
- A defensible candidate headline whose stability is documented.

## What failure looks like

- Stability share <50%. The result is fragile. Investigate which specs disagree with the candidate headline; the disagreement is informative (often it points to a sample-selection problem or an FE that absorbs the treatment).
- The candidate headline has p=0.04 but the median spec has p=0.30. You are about to spec-mine. The honest move is to disclose that the result is not robust to FE choice.
- The grid is run but `exploration_log.md` is empty. Run the grid AND write the log in the same session, otherwise you will forget what you did.

## Pitfalls

- Running the grid but only saving the favorable spec to the table file. The grid is the truth; the table is one row from the grid.
- Adding more specs to the grid until you find one with p<0.05. This is spec-mining. The grid is fixed before you run it; you can extend with documented additions but you cannot prune retroactively.
- Choosing the headline by p-value rather than by stability. Stability is the right criterion. A spec with p=0.06 and grid-median coefficient is better than a spec with p=0.01 and outlier coefficient.

## Spawn parallel for speed

If the grid is large, spawn 3-4 subagents in parallel:
- Subagent 1: runs the OLS specs across outcomes/samples/FE.
- Subagent 2: runs the same grid with alternative SE (province-industry double-cluster, wild bootstrap).
- Subagent 3: runs the event-study version (staggered DiD: Callaway-Sant'Anna, Sun-Abraham) on the candidate headline.
- Subagent 4: runs the heterogeneity (by SOE, by Big-4, by firm size tercile).

## Time budget

1-3 days. The grid itself runs in minutes; reading and documenting takes the time.
