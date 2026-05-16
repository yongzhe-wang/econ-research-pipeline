# Failure Modes — Six Hard-Won Lessons

These are real failures the originating project hit. Each entry has a description, a detection test, and a recovery procedure.

---

## 1. Mean reversion masquerading as policy effect

### What happened

In the original project, a 2019 VAT-rate-cut DID on book-tax differences (BTD) looked publishable: clean parallel trends, significant post-treatment effect, robust to several FE structures.

The placebo-quartile test killed it. The "effect" concentrated in firms with extreme pre-treatment BTD: top quartile reverted downward, bottom quartile reverted upward, middle quartiles were flat. The treatment effect was symmetric reversion to the mean, not a causal response to the VAT cut.

### How to detect

Run the placebo-quartile test:

```python
# For each treated unit, compute pre-treatment outcome mean
pre_Y = df[df["pre_treatment"]].groupby("unit_id")["Y"].mean()
df["pre_Y_q"] = pd.qcut(df["unit_id"].map(pre_Y), 4, labels=[1,2,3,4])

# Re-estimate within each quartile
for q in [1, 2, 3, 4]:
    sub = df[df["pre_Y_q"] == q]
    m = pf.feols("Y ~ T | unit + year", data=sub)
    print(q, m.coef()["T"], m.se()["T"])
```

If top and bottom quartiles show opposite-sign coefficients of similar magnitude, the result is mean reversion. Stop and reframe.

### How to recover

- Reframe the result as descriptive, not causal. "Firms with extreme pre-treatment BTD revert toward the mean post-policy" is a valid descriptive finding; it is not a treatment effect.
- Or drop the result entirely and look for a different identifying source of variation.
- Document the kill in `dead_branches.md`.

---

## 2. Endogenous selection in switch events

### What happened

The original project used auditor-pair switches (from FF to non-FF) as the treatment. The naive within-firm spec aggregated all switches together. The treatment variable was contaminated by client-initiated switches (firms that drop their auditor specifically because they want a more lenient one).

### How to detect

Build the full transition matrix:

```python
tm = pd.crosstab(df["prior_state"], df["current_state"])
print(tm)
```

For each cell, compute the within-cell coefficient. If one cell drives the headline disproportionately, the treatment is really about that cell, not the broader category.

Also: decompose switches by exogeneity. PRC 5-year mandatory rotation is plausibly exogenous; client-initiated voluntary changes are not. Run the spec restricted to plausibly exogenous switches separately.

### How to recover

- Decompose by switch type. Report each subsample with N, coefficient, SE.
- For the headline, use the subsample that is most plausibly exogenous (or pool with explicit weighting).
- If the exogenous subsample fails HonestDiD (real example: PRC rotation failed at M-bar=0.002), demote to corroboration.

---

## 3. Cross-section selection bias

### What happened

Initial gender-effect specs in the original project compared firms with FF auditor pairs to firms with non-FF auditor pairs cross-sectionally. The cross-section showed a significant relationship. The within-firm event study (FF-loss within the same firm) was nearly null.

The cross-section was selection: firms that hire FF pairs are different from firms that hire non-FF pairs along many unobserved dimensions.

### How to detect

If you have a cross-section result, also run the within-firm version. If they disagree by more than 1 SE, the cross-section is largely selection.

```python
# Within-firm: keep only firms with within-firm variation in treatment
firms_with_variation = df.groupby("firm")["T"].nunique()
firms_with_variation = firms_with_variation[firms_with_variation > 1].index
within = df[df["firm"].isin(firms_with_variation)]
m_within = pf.feols("Y ~ T | firm + year", data=within)
```

### How to recover

- Lead with the within-firm result. Mention the cross-section briefly with explicit selection-bias caveats.
- Or reframe the paper as documenting the cross-sectional pattern and exploring its sources (selection vs. treatment), with explicit IV / matching attempts.
- The original project shifted to asymmetric framing (FF-loss vs FF-gain) — within-firm event study with falsification arm — which recovered real signal.

---

## 4. Bundled treatment

### What happened

The auditor-switch event in the original project often co-occurred with other corporate changes: CFO turnover, audit-firm change, audit-fee shock, Big-4 status change. A naive spec attributing the BTD effect to gender alone would mis-credit.

### How to detect

Add contemporaneous controls in {t-1, t, t+1} for every plausibly co-occurring event:

```python
controls = []
for var in ["cfo_change", "big4_change", "fee_shock", "ownership_change"]:
    for lag in [-1, 0, 1]:
        df[f"{var}_lag{lag}"] = df.groupby("firm")[var].shift(-lag)
        controls.append(f"{var}_lag{lag}")

formula = f"Y ~ T + {' + '.join(controls)} | firm + year"
m = pf.feols(formula, data=df, vcov={"CRV1": "firm"})
```

If the headline coefficient halves after adding bundled controls, the bundled-controls coefficient is the honest headline.

### How to recover

- Use the bundled-controls coefficient as the headline.
- Report both versions (with / without bundled controls) so the reader sees the partial-r2 of the focal treatment vs. the bundled events.
- The original project's coefficient was stable: +0.0089 without bundled controls, +0.0088 with them, suggesting the treatment was not bundled. But you only know this by running the test.

---

## 5. HonestDiD failure (parallel-trends sensitivity)

### What happened

The PRC mandatory partner rotation subsample looked like the cleanest causal slice in the original project (clients cannot choose to rotate — the regulator forces it). The headline coefficient was significant.

HonestDiD (Rambachan-Roth 2023) showed the 95% CI crossed zero at M-bar ≈ 0.002 — meaning even tiny deviations from parallel trends would void the result. The subsample is not causal in the strict sense.

### How to detect

Run HonestDiD. Report the full M-bar curve.

```r
library(HonestDiD)
out <- createSensitivityResults(
  betahat = es$coef, sigma = vcov,
  numPrePeriods = 4, numPostPeriods = 4,
  Mbarvec = c(0, 0.5, 1, 1.5, 2)
)
```

Look at the M-bar at which the CI first crosses zero. If <1, the result is fragile under modest parallel-trends violations.

### How to recover

- If M-bar < 1: demote to corroboration. Report the full curve in the main paper, not the appendix. Be honest in prose: "the rotation subsample provides corroborative evidence; under Rambachan-Roth sensitivity it does not survive M-bar < X".
- Use a different subsample for the headline. Find one whose M-bar survives at 1+.
- Or restructure the paper around what does survive (within-firm event study survived in the original project).

---

## 6. Citation fabrication by LLM

### What happened

Six fabricated citations in the original project, across multiple rounds. Patterns:
- Real authors paired with non-existent papers ("Aobdia and Smith 2024" — Aobdia is real, Smith is real, the joint paper does not exist).
- Real paper attributed to wrong year (Aobdia 2017 cited as Aobdia 2019, with a slightly different finding line).
- Real journal name with non-existent volume / pages.
- Plausible-sounding paper that has no Crossref hit and no Google Scholar hit.

### How to detect

The verification protocol (Phase 8): for each citation, resolve DOI via Crossref or publisher page; read abstract; confirm finding line.

Red flags during verification:
- "Forthcoming" with no volume / no SSRN URL.
- Co-author pairs that have never collaborated.
- Page ranges with implausible spans (4-page AER article).
- Journal name slightly off.
- DOI does not resolve.

### How to recover

- Delete the entry. Find a replacement that covers the same claim. Verify the replacement.
- Document the fabrication in `dead_branches.md` with the fake citation.
- For each fabrication found, the question is: did this paper appear in any prose? If yes, the prose claim now needs re-sourcing from a real paper. Update the prose; do not just swap the bib entry.
- If the fabrication was used as a key citation for the main contribution, the contribution claim needs re-examination. Maybe the literature does not actually leave the gap you thought.

### Why this matters

A fabricated citation in a published paper is a career-affecting event. Reviewers spot-check. Discussants spot-check. Replicators spot-check. The protocol is mandatory precisely because the cost of one undetected fabrication is high.

---

## How to use this list

At the start of every paper, read this file. The six failures are the most common; not the only ones. As you hit new failures in your own projects, document them at the bottom of this file. Over time, the failure list becomes a research-discipline checklist for your group.

Most failures share a structure: a result that looked clean until a specific test was run. The pipeline's Phase 4 (killer tests) and Phase 8 (citation verification) exist because these failures kept happening. The detection tests are not optional.
