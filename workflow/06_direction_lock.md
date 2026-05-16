# Phase 5 — Direction Lock

## Objective

Commit to a headline. Cut what did not survive Phase 4. Update `goal.md` and `design.md` so they reflect the locked direction, not the original wish.

## Inputs

- Phase 4 killer-test results.
- Original `goal.md` and `design.md`.

## Outputs

- Updated `goal.md` (RQ may be sharpened; hypotheses now match what survived).
- Updated `design.md` (estimating equation, sample, identification threats, robustness queue).
- `dead_branches.md` — for honest record-keeping, what was tried and killed.
- One-paragraph locked-direction statement, human-approved.

## Steps

### Step 1 — Write the locked-direction paragraph

One paragraph, five sentences, plain English:

```
The paper studies [RQ in one sentence]. The identification exploits
[treatment variation source]. The headline coefficient is [beta, SE,
N, p]. The binding falsification is [the asymmetric arm / placebo / etc.].
Parallel trends survive Rambachan-Roth sensitivity through M-bar = [X].
```

Example from the original project:

```
The paper studies whether the within-firm dissolution of an all-female
signing-auditor pair raises reported book-tax differences at Chinese
A-share firms. The identification exploits within-firm transitions
from FF to non-FF pairs, with the PRC 5-year mandatory rotation
subsample as corroboration. The headline coefficient is +0.0089
(SE 0.0032, p = 0.006, N = 20,306), representing about 25 percent
of the BTD mean. The binding falsification is the null gain arm
(FM/MF -> FF, beta = +0.0012, p = 0.61), ruling out symmetric
mean reversion. Parallel trends survive Rambachan-Roth sensitivity
through M-bar ≈ 0.002.
```

### Step 2 — Human approval

Read the paragraph back to the human. Confirm:
- The RQ matches what the data supports.
- The headline coefficient is the one that survived Phase 4.
- The binding falsification is the right falsification (not a weaker one chosen because the right one fails).
- The HonestDiD threshold is reported honestly.

If the human disagrees, iterate. Do not move to Step 3 without explicit approval.

### Step 3 — Update goal.md

Sharpen the RQ. Update the hypothesis block so each pre-specified hypothesis is now resolved (supported / rejected / null).

Example update format:

```markdown
## Hypotheses (pre-specified, resolved Phase 5)

- H1 (main): FF-loss raises BTD at t=0. **Supported.** beta = +0.0089 (SE 0.0032, p = 0.006).
- H2 (persistence): Effect persists at t=+1. **Supported.** beta = +0.0083 (SE 0.0035, p = 0.018).
- H3 (specificity): MM-loss is null. **Supported.** beta = +0.002 (p = 0.39).
- H4 (asymmetry): FF-gain does not lower BTD. **Supported.** beta = +0.0012 (p = 0.61).
- H5 (mandatory rotation): Survives in PRC rotation subsample. **Rejected at M-bar=0 in HonestDiD.** Demoted to corroborative descriptive evidence.
```

Honesty here pays. Reviewers can spot a paper whose hypotheses were rewritten to match results; the pre-spec version with explicit resolution is the credible report.

### Step 4 — Update design.md

The estimating equation, sample definition, controls, FE structure, and SE clustering all stay in `design.md`. Update them to match the locked spec from Phase 3 / 4.

Add a new section "Identification threats with mitigations" listing every threat Phase 4 surfaced and the test that addresses it.

Add a "Robustness queue" — every test you will run in Phase 9 (full paper draft). This becomes the checklist for the robustness section.

### Step 5 — Write dead_branches.md

Honest record of what was tried and did not work. Not for the paper; for your own files and for transparency if questioned.

Format:

```markdown
# Dead branches

## PRC mandatory rotation as headline
Tried as cleanest causal slice. Failed HonestDiD at M-bar ≈ 0.002.
Demoted to corroboration in main text; full HonestDiD curve in appendix.

## 2019 VAT DID
Looked publishable until placebo-quartile test showed mean reversion
explains it (top BTD quartile reverted down, bottom reverted up).
Dropped entirely.

## Cross-section gender effect
Initial within-firm spec was null on the simple FF dummy. Signal
appeared only in switch events with asymmetric framing. The original
cross-section spec is in the appendix as a null result.

## Mediation through audit fee
Predicted that the FF-loss effect would partly route through changes
in audit fee. Mediation = 8.4%, p = 0.41, not significant. The
mechanism in the main text is "silent monitoring", net of audit fee.
```

This file does not go in the paper; it lives in the paper folder for your records.

## What success looks like

- One paragraph locked-direction statement, human-approved.
- `goal.md` updated with resolved hypotheses.
- `design.md` updated with locked estimating equation and robustness queue.
- `dead_branches.md` written for honesty.

## What failure looks like

- Locked direction includes a result that did not actually survive Phase 4. Go back. Phase 4 is the gate.
- Locked direction is vague ("we study auditor characteristics and tax outcomes"). Tighten to specific coefficient + binding falsification.
- The human did not actually approve, the assistant moved ahead anyway. Stop. Reload the STOP gate norm.

## Pitfalls

- Reframing a dead result as alive ("PRC rotation provides supportive evidence" when HonestDiD killed it). The honest version is "we tested PRC rotation; it does not survive parallel-trends sensitivity at M-bar=0; we report this in the appendix."
- Updating the hypothesis block to match results without acknowledging the change. If H1 was "negative" originally and the data shows positive, write "H1 (pre-specified): negative. Resolved: rejected; observed sign is positive. We discuss the reframing in Section X."
- Skipping `dead_branches.md` because it feels embarrassing. It is the most useful file in the folder — it is the audit trail for your own honesty.

## Time budget

Half a day. The thinking is done; this phase is documentation discipline.
