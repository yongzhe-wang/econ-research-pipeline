# Example — FF-Loss and Tax Avoidance

This example shows a complete run-through of the 10-phase pipeline on a real Chinese A-share auditor characteristics x tax avoidance project. The actual paper is private; the workflow artifacts are open and anonymized to the extent any specific institutional details are obscured.

The point of the example: someone setting up the pipeline for the first time can read these three files to see what a realistic `goal.md`, `design.md`, and headline-results summary look like at the end of Phase 5 (direction lock).

## Files

- `goal.md` — Phase 0 RQ, data inventory, pre-specified hypotheses, with Phase 5 hypothesis resolutions added.
- `design.md` — Phase 2/4 estimating equation, sample definition, identification threats, robustness queue.
- `headline_results.md` — Phase 4 / 5 summary: headline coefficient, binding falsification, HonestDiD breakdown, mechanism null, sample sizes.

## What this example demonstrates

1. **Pre-specified hypotheses with explicit resolution.** Five hypotheses pre-specified at Phase 0. Phase 5 marks each as Supported / Rejected / Null. H5 was rejected by HonestDiD; the example honestly reports the rejection rather than reframing.

2. **Asymmetric falsification arm as binding test.** The headline (FF-loss raises BTD) is paired with the null gain arm (FF-gain does not lower BTD). The asymmetry is what rules out mean reversion.

3. **HonestDiD demoting a subsample.** The PRC mandatory partner rotation subsample looked like the cleanest causal slice. HonestDiD showed it does not survive M-bar ≈ 0.002. The example documents the demotion to corroboration.

4. **Mechanism null with honest framing.** The mediation through audit fee was 8.4%, p = 0.41. Not significant. The example writes "silent monitoring net of fees" rather than overclaiming an audit-fee channel.

## What this example does NOT include

- The full paper PDF (private).
- The raw data file (CSMAR licensed; not redistributable).
- The full code (also private, partly because it contains data-vendor specific cleaning steps).
- The complete annotated bibliography (private; would dox specific work).

If you want to use this example as a template, copy the structure of `goal.md`, `design.md`, and `headline_results.md` into your own paper folder and replace the content.

## How this fits the pipeline

This artifact set is the output of Phase 5 (direction lock). Specifically:

- `goal.md` was first drafted at Phase 0 (with pre-spec hypotheses) and updated at Phase 5 (with resolutions).
- `design.md` was first drafted at Phase 2 (post-litreview design) and updated at Phase 4/5 (with locked spec and robustness queue).
- `headline_results.md` was written at Phase 5 as the locked-direction statement.

Phase 6 (exemplar collection) and onward use these artifacts as inputs.
