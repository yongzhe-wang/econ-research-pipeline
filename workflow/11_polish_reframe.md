# Phase 10 — Polish and Honest Reframing

## Objective

Surface every weak claim. Reframe overclaims to defensible language. Final stylistic pass. Final compile.

## Inputs

- First full draft from Phase 9.

## Outputs

- Polished `draft/paper.tex`.
- Final `draft/paper.pdf`.
- `results/polish_audit.md` — list of changes made, with reasons.

## Steps

### Step 1 — Codex full-draft red-team

Run Codex one more time on the full draft.

```bash
codex exec <<'EOF'
You are reviewing the final draft of an econ paper. Find every claim
that is not directly supported by a number in the results section.
Find every place where the language overstates what the evidence
shows. Find every place where a critical hedge is missing.

Here is the draft:

[paste paper.tex]

Return your verdict in this format:

## Unsupported claims
- Line <N>: "<quoted claim>" — supported by <table/figure>? <answer>.

## Overstatements
- Line <N>: "<quoted claim>" — defensible version: "<rewrite>".

## Missing hedges
- Line <N>: "<quoted claim>" — needs hedge because <reason>; suggested:
  "<rewrite>".
EOF
```

Save to `results/codex_polish_review.md`.

### Step 2 — Honest reframing

For each Codex-flagged item, rewrite to the defensible version.

Examples of overclaims and their defensible rewrites:

| Overclaim | Defensible rewrite |
|-----------|---------------------|
| "We establish a causal effect of X on Y." | "We identify a within-firm change in Y associated with X-loss events, robust to bundled-treatment controls and asymmetric framing." |
| "The mechanism is monitoring." | "Patterns are consistent with monitoring; the mediation through audit fee is 8.4% (p = 0.41), so the headline channel must be silent monitoring not priced through fees." |
| "Our results generalize to all listed firms." | "We document the effect in Chinese A-share firms 2017-2024; whether it generalizes to other regulatory regimes is left to future work." |
| "We close a gap in the literature." | "Prior work documents firm-level auditor effects (cite); our contribution is the individual-auditor demographic channel within firm, with an explicit asymmetric-arm falsification." |

### Step 3 — Style pass

Run through the full text. Apply the style guide from `templates/style.md`:

1. **Drop LLM tells.** Find-replace these phrases throughout:
   - "delve into" → "examine" or just delete
   - "leverage" → "use"
   - "navigate" → "address" or delete
   - "in the realm of" → "in"
   - "it is important to note that" → delete; just state the thing
   - "moreover" / "furthermore" as paragraph openers → use direct logical connection or delete
   - "this paper aims to" → "this paper estimates / documents / tests"

2. **Drop hedges.** Find these and decide if each is load-bearing or vapor:
   - "arguably" → delete unless you mean it
   - "it seems" → delete; state directly
   - "one might suggest" → delete; whose suggestion?
   - "interestingly" → delete; if it is interesting let the reader see why

3. **Numbers before adjectives.** Find patterns like "a meaningful increase" and replace with "0.9 pp (25% of mean)".

4. **Effect direction in plain English.** Find "the coefficient on T is 0.042" and rewrite as "raises wages by 4.2%".

### Step 4 — Section coherence pass

Read each section aloud (or with a TTS tool) start to end. Look for:
- Repeated sentences across sections (intro and abstract often duplicate; pick one phrasing, vary the other).
- Subsection headers that promise more than the subsection delivers.
- Tables referenced in prose that do not appear nearby (move tables closer or use `\ref` properly).
- Citation overload (more than 4-5 citations in a single parenthetical is rarely useful).

### Step 5 — Final compile and inspection

```bash
cd draft/
tectonic paper.tex
```

Open the PDF. Inspect:
- Title page, abstract, JEL codes (if needed).
- Table of contents (if used; most working papers do not).
- Section headers match.
- Tables render correctly (no overflow, FE rows labeled).
- Figures render correctly.
- Bibliography is alphabetized by author, complete.
- Page count is reasonable for target journal.

### Step 6 — Write the polish audit

`results/polish_audit.md` summarizes changes:

```markdown
# Polish audit

## Unsupported claims fixed
- Line 142 "we establish a causal effect" -> "we identify a within-firm change associated with..."
- Line 287 "the mechanism is..." -> "patterns are consistent with..., mediation through... is null..."

## LLM tells removed
- "delve into" x 3 occurrences
- "leverage" x 2 (kept 1 where it meant "financial leverage")
- "it is important to note" x 1
- "moreover" as paragraph opener x 4

## Hedges audited
- "arguably" x 1 deleted (vapor)
- "interestingly" x 2 deleted
- "consistent with" x 6 retained (load-bearing for honesty)

## Numbers / adjectives
- "a meaningful increase" -> "0.9 pp (25% of mean)" x 3
- "a substantial drop" -> "1.4 pp (40% of mean)" x 2
```

This audit is a checkpoint. If the next reviewer asks "what did you change in polish?", the audit answers.

## What success looks like

- Final PDF compiles clean.
- Codex returns no remaining unsupported claims.
- Every overclaim has been rewritten or its evidence has been added to results.
- The style pass has been applied uniformly.
- The audit is documented.

## What failure looks like

- "Codex flagged 12 unsupported claims; I fixed 6 and ignored 6." Fix all 12 or document each ignore with a counter-argument.
- "I removed all hedges to make the paper sound stronger." Hedges that are load-bearing (HonestDiD threshold, mechanism null) MUST stay. Removing them is overclaim.
- "The intro contradicts the conclusion." Re-read both. Fix.

## Pitfalls

- Polish-as-cosmetic without addressing substantive overclaims. The substantive review (Step 1-2) is the most important; the style pass (Step 3) is secondary.
- Adding new analysis at this phase. Do not. New analysis is revision; polish is honesty + style.
- Cutting hedges that are load-bearing. "Consistent with X" is honest when you have correlational evidence; "X causes Y" is overclaim. Do not collapse the distinction.

## After Phase 10

The paper is ready for the next step: human review by your advisor / co-authors, then submission. The pipeline ends here.

When reviewers come back with comments, the standard pattern is:
- Comment requests new analysis -> new code, new results, update relevant section.
- Comment requests reframing -> Phase 5-style direction-lock update.
- Comment requests additional lit review -> Phase 1-7 mini-cycle on the specific gap they flagged.
- Comment requests robustness check -> Phase 4-style killer test, added to robustness section.

## Time budget

3-5 days. Codex review and reframing are fast (1-2 days). Style pass is slow because it requires reading every sentence (2-3 days). Final inspection is half a day.
