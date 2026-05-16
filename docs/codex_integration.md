# Codex Integration — Cross-Model Red-Team Recipe

## Why cross-model

The pipeline uses a 3-channel calibration model:

- **Channel A**: main Claude (the primary drafting assistant).
- **Channel B**: blind subagent — same model family, fresh context. Used for fresh-eye review of specific artifacts (e.g., a draft section).
- **Channel C**: a different model entirely. Used to catch failure modes specific to model A.

Channel C is the strongest signal. If two different model families agree, you have triangulated. If they disagree, you have a real question to investigate.

The reference Channel C in this pipeline is OpenAI Codex CLI running GPT-5.5. Codex catches things Claude misses:
- Different training data emphasis (different journal coverage).
- Different hallucination patterns (Codex fabricates differently from Claude; a citation that survives both is more likely real).
- Different econometric blind spots.

## When to invoke Codex

Three mandatory points:

1. **After Phase 1 (lit review).** Ask Codex if the gap is real, if the angle is novel, if there are competitor papers Claude missed.
2. **After Phase 4 (killer tests).** Ask Codex if there is any identification threat that survived the kill tests.
3. **Before final submission (Phase 10).** Ask Codex to find unsupported claims, overstatements, missing hedges.

Optional but recommended:

4. **After Phase 8 (citation verification).** Ask Codex to verify a sample of 5 random citations as a cross-check (catches fabrications Claude might have missed).

## Codex prompt anatomy

Codex CLI runs one-shot prompts by default. The prompt must be self-contained: include ALL context Codex needs (no follow-up turns).

Template:

```bash
codex exec <<'EOF'
ROLE: You are a senior econ paper red-team reviewer.

TASK: <one sentence on what you want Codex to do>

CONTEXT:
[paste the relevant artifact: annotated_bib.md, design.md, paper draft, etc.]

OUTPUT FORMAT:
## <section 1>
- <item>: <details>

## <section 2>
- <item>: <details>

CONSTRAINTS:
- Be specific. Name papers by author + year + journal.
- Be skeptical of any claim that lacks evidence in the artifact.
- Do not suggest stylistic changes; focus on substance.
EOF
```

## Phase 2 — Post-litreview red-team

```bash
codex exec <<'EOF'
ROLE: You are an econ paper red-team reviewer.

TASK: Critique my Phase 1 lit review and three candidate contribution
angles. (a) Find papers I missed from 2022-2026 that cover the same
ground. (b) Flag any angle that has a published competitor I did not
cite. (c) Critique each angle's identification strategy with a specific
failure mode (mean reversion, selection, bundled treatment, etc.).

CONTEXT:
== goal.md ==
[paste]

== annotated_bib.md ==
[paste]

OUTPUT FORMAT:
## Missed papers
- <full citation>: <one sentence on why it matters>

## Competitor flag
- Angle <A/B/C>: <specific competitor paper> covers <what>; my
  differentiation needs to be <what>.

## Identification critique
- Angle <A/B/C>: <specific failure mode>, mitigation: <specific test>.

## Suggested additional robustness
- Angle <A/B/C>: <test 1>, <test 2>.

CONSTRAINTS:
- Name papers by author + year + journal + DOI when possible.
- If you cite a paper, be willing to commit to its DOI being correct.
- Distinguish strong from weak signals: mark each item as
  HIGH / MEDIUM / LOW confidence.
EOF
```

Save output to `results/codex_redteam_phase1.md`.

## Phase 4 — Post-killer-test red-team

```bash
codex exec <<'EOF'
ROLE: You are an econ paper red-team reviewer.

TASK: Critique the killer tests I ran. Did I miss a threat? Is there a
specific failure mode that survived these tests but could still bite me?

CONTEXT:
== design.md (locked direction) ==
[paste]

== Killer test results summary ==
- Placebo-quartile test: [results]
- Transition matrix decomposition: [results]
- Asymmetric framing: [results]
- HonestDiD: [results, with M-bar curve]
- Within-firm event study: [results]
- Bundled-treatment controls: [results]

OUTPUT FORMAT:
## Threats that survived these tests
- <threat>: <why these tests do not address it>

## Suggested additional tests
- <test>: <what it would address>

## Interpretation flags
- <claim in design.md>: <issue with phrasing or scope>
EOF
```

Save to `results/codex_redteam_phase4.md`.

## Phase 8 — Citation cross-check (sample)

After the bulk verification protocol, pick 5 random entries and ask Codex:

```bash
codex exec <<'EOF'
ROLE: You are a citation verifier.

TASK: For each of the following 5 bib entries, confirm whether the paper
exists by resolving the DOI / publisher page / SSRN URL. If any entry
appears to be fabricated, flag it.

CONTEXT:
[paste 5 verified entries with their citations and DOIs]

OUTPUT FORMAT:
## Entry 1 — <citation>
DOI: <provided>
- Crossref / publisher hit: YES / NO
- Authors match: YES / NO
- Year match: YES / NO
- Verdict: VERIFIED / FLAGGED / UNRESOLVED

## Entry 2 ...
...
EOF
```

If Codex flags any "verified" entry as unresolved, dig deeper. Either Codex is wrong (it does happen) or your verification missed something.

## Phase 10 — Final-draft red-team

```bash
codex exec <<'EOF'
ROLE: You are a senior econ paper reviewer at a top-5 journal.

TASK: Find every claim in the draft that is not directly supported by a
number in the results section. Find every place where the language
overstates what the evidence shows. Find every place where a critical
hedge is missing.

CONTEXT:
== paper.tex (full draft) ==
[paste]

OUTPUT FORMAT:
## Unsupported claims
- Line <N>: "<quoted claim>" — supported by <table/figure>?
  Verdict: SUPPORTED / WEAK / UNSUPPORTED.

## Overstatements
- Line <N>: "<quoted claim>" — defensible version: "<rewrite>".

## Missing hedges
- Line <N>: "<quoted claim>" — needs hedge because <reason>;
  suggested: "<rewrite>".

## Internal consistency issues
- <issue>: where intro claims X, results show Y.

CONSTRAINTS:
- Be specific to line numbers in the LaTeX source.
- Be ruthless on overclaims; the cost of an overclaim is higher than
  the cost of an extra hedge.
EOF
```

Save to `results/codex_polish_review.md`. Triage every flagged item in Phase 10.

## Codex's blind spots

Codex has known weaknesses; supplement with manual work in these areas:

1. **Recent papers (last 6 months).** Training cutoff. Manual Google Scholar / arXiv search.
2. **Chinese-language papers.** Limited training coverage. Manual CNKI search.
3. **Very specialized econometric details.** Codex is strong on canonical methods (DiD, IV, RDD) and weak on niche refinements. Cross-check methodological claims against the original paper, not Codex.
4. **Author-name spelling for less-famous authors.** Codex may guess on spelling. Verify against publisher pages.

## What Codex is great at

1. **Sniffing out unsupported claims.** Strong pattern-matcher for "this claim has no number".
2. **Identifying competitor papers in English-language top journals.** Strong coverage of AER, QJE, JPE, ReStud, Econometrica, top field journals through ~2024.
3. **Critiquing identification strategies.** Strong at naming specific failure modes given a research design.
4. **Catching internal inconsistency.** Strong at "intro says X, results say Y" type mismatches.

## When to disagree with Codex

Codex is not always right. Reasonable grounds to push back:

- Codex flags a paper as a competitor; on reading, it covers a different sample / different ID. Document the difference in `results/codex_redteam_phaseN.md` and proceed.
- Codex flags an identification threat; on reflection, your design already addresses it via a specific robustness check. Note the check; proceed.
- Codex suggests a hedge that is not load-bearing. Use judgment.

Document every push-back in writing. The audit trail matters if a reviewer later asks "what about X?".

## Fallback if Codex CLI is unavailable

The principle is cross-model, not Codex specifically. Acceptable substitutes:

- Gemini via Google AI Studio.
- GPT-5 via OpenAI web UI (copy-paste).
- Claude (different model family — Claude Sonnet vs Claude Opus) — weaker signal because same family, but better than nothing.

If only one model family is available, run Channel B (blind subagent, fresh context) and explicitly flag that you skipped Channel C in your project log.
