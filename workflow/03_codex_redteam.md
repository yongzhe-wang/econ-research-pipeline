# Phase 2 — Codex Cross-Model Red-Team

## Objective

Use a second model (Codex CLI / GPT-5.5) to red-team the Phase 1 lit review. Catch competitor papers we missed. Catch fatal identification flaws in the three proposed angles. Channel C of the 3-channel calibration model.

## Inputs

- `annotated_bib.md` from Phase 1.
- `goal.md`.

## Outputs

- `results/codex_redteam_phase1.md` — Codex's verdict.
- Updated `annotated_bib.md` (if Codex finds papers we missed, add them).
- Possibly: one of the three angles dropped or substantially revised.

## Why cross-model

The 3-channel calibration model:
- **Channel A**: main Claude (your primary assistant; the one drafting).
- **Channel B**: blind subagent — same model family, fresh context. Used for fresh-eye review of a specific artifact.
- **Channel C**: different model entirely. Used to catch failure modes specific to model A.

Channel C is the strongest signal. If two different model families agree, you have triangulated. If they disagree, you have a real question to investigate.

For econ research the typical channel C is OpenAI's frontier (GPT-5.5 via Codex CLI). It catches things Claude misses:
- Different training data emphasis (e.g., different coverage of certain journals).
- Different hallucination patterns (Codex fabricates differently from Claude, so a citation that survives both is more likely real).
- Different blind spots — Codex is worse on Chinese-language papers, better on certain econometric details.

## The Codex prompt

Codex CLI is one-shot by default. Construct a self-contained prompt with all needed context.

```bash
codex exec <<'EOF'
You are an econ paper red-team reviewer. I have a draft lit review and
three candidate contribution angles. Your job is to:

1. Identify papers I missed from 2022-2026 that cover the same ground.
2. Flag any angle that has a published competitor paper I did not cite.
3. Critique each angle's identification strategy with a specific failure
   mode (mean reversion, selection, bundled treatment, etc.).
4. Suggest two additional robustness checks per angle.

Here is my draft lit review and angles:

[paste contents of annotated_bib.md]

Here is my goal.md:

[paste contents of goal.md]

Return your verdict in this format:

## Missed papers
- <citation>: <why it matters>

## Competitor flag
- Angle <A/B/C>: <specific competitor paper> covers <what it covers>;
  my differentiation needs to be <what>.

## Identification critique
- Angle <A/B/C>: <specific failure mode>, mitigation: <specific test>.

## Additional robustness
- Angle <A/B/C>: <test 1>, <test 2>.
EOF
```

Save the output to `results/codex_redteam_phase1.md`.

## Triage Codex's verdict

For each flagged item:

1. **Missed paper.** Read it. If it covers your angle, you need to either differentiate or drop. If it does not actually cover the angle (Codex misjudged), document why in `results/codex_redteam_phase1.md` with a note.

2. **Competitor flag.** Same as above. Often the "competitor" turns out to be on a related but distinct topic. The honest move is to add it to lit review with a one-sentence differentiation.

3. **Identification critique.** Take seriously. Codex is often right about identification because the failure modes are well-documented in textbooks. Add the suggested robustness test to your `design.md` queue.

4. **Additional robustness.** Add to the queue; run in Phase 3.

## What to ignore from Codex

- Comments on Chinese-language papers it cannot verify (training cutoff, transliteration issues).
- Comments about papers published in the last 6 months (training cutoff).
- Style suggestions ("rephrase X"). Codex's style differs from yours; the style guide is `style.md`, not Codex.

## STOP gate

Update `annotated_bib.md` with missed papers. Update or drop angles per identification critique. Then move to Phase 3.

## What success looks like

- Every Codex-flagged competitor is either incorporated or rebutted with evidence.
- Every Codex-flagged identification threat has a corresponding robustness check in `design.md`.
- `results/codex_redteam_phase1.md` is complete and reviewed by the human.

## What failure looks like

- "Codex flagged 3 papers; I read 1 and skipped the other 2 because they looked unrelated from the title alone." This is the failure mode the protocol exists to prevent. Read all flagged papers' abstracts at minimum.
- "Codex's identification critique is wrong, so I'll ignore it." Maybe. But document why with a specific counter-argument. Saying "Codex is wrong" without a counter is not a rebuttal.
- "Codex confirmed my angle is novel." Codex agreeing is not strong evidence (it could be missing the competitor too). Codex disagreeing is strong evidence (cross-model false-negative is rare).

## Pitfalls

- Treating Codex's training cutoff as universal. Codex's knowledge of recent (last 6 months) papers is thin. Supplement with manual Google Scholar / arXiv search.
- Treating Codex as adversarial bickering. It is calibration channel C, not an opponent. Engage with it the way you would engage with a thoughtful seminar discussant.
- Pasting only the gap analysis without the full bib. Codex cannot judge differentiation without seeing what you already cite.
- Skipping Codex because "I am confident in my angle". Confidence is exactly when calibration matters most.

## Fallback if Codex CLI is unavailable

Use another available frontier model: Gemini, GPT-5 web UI, etc. The principle is cross-model, not Codex specifically. If only Claude is available, run channel B (blind subagent, fresh context) and explicitly flag that you skipped channel C.

## Time budget

1-3 hours. Codex prompt construction takes 10 minutes; the run is fast (5-15 minutes); triage takes 1-2 hours.
