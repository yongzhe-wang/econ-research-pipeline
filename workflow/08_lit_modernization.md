# Phase 7 — Literature Review Modernization

## Objective

Push at least 80% of cited references to 2020 or later. Replace pre-2020 papers with current equivalents where possible. Keep only the genuinely foundational pre-2020 papers.

## Inputs

- `annotated_bib.md` from Phase 1 (possibly updated through Phases 2 and 6).

## Outputs

- Updated `annotated_bib.md` with ≥80% post-2020 share.
- A short note at the top of `annotated_bib.md` listing the retained pre-2020 papers with one-line justifications.

## Why 80% post-2020

The literature in any active field doubles every 5-7 years. A paper whose lit review is half pre-2020 is signaling "I am not current". Reviewers in 2026 will look for engagement with 2024-2026 work, especially methodological work (HonestDiD, Callaway-Sant'Anna refinements, modern DML).

Pre-2020 papers stay only when:
- They are the canonical / foundational paper for the field (e.g., Card 1990; DellaVigna and Gentzkow 2019; Hsieh and Klenow 2009).
- They are the original method paper for a still-used method (e.g., Imbens and Angrist 1994 for LATE; Callaway and Sant'Anna 2021 is borderline because 2021 is recent enough).
- They are the canonical empirical paper for a phenomenon you are extending (e.g., Faulkender and Petersen 2006 if you are doing a tax-aggressiveness paper that builds directly on their setup).

## Recipe

### Step 1 — Compute current share

```python
import re
with open("annotated_bib.md") as f:
    text = f.read()
years = [int(y) for y in re.findall(r"\((20\d\d)\)", text)]
post = sum(y >= 2020 for y in years) / len(years)
print(f"Post-2020 share: {post:.1%}")
```

If ≥80%, skip Phase 7. If <80%, proceed.

### Step 2 — List every pre-2020 paper with justification

For each pre-2020 paper in the bib, write a one-line justification:

```
- Card 1990 (AER) — FOUNDATIONAL: original Mariel boatlift paper, canonical
  reference for labor-supply natural experiments.
- DeAngelo 1981 (JFE) — TO REPLACE: argument is now subsumed by Faulkender
  2024 (JF). Find Faulkender 2024 and replace.
- Khan and Lu 2013 (JAR) — TO REPLACE: BTD-tax-avoidance correlation, 
  superseded by Chen 2024 (TAR). Find and replace.
```

### Step 3 — Replace where possible

For each "TO REPLACE":

1. Find the modern replacement. Use Semantic Scholar's "cited by" graph from the old paper; the most-cited 2022-2026 descendant is usually the modern reference.
2. Verify the replacement actually covers the same point (it often refines or extends; if it refines, the framing in your paper may need a tweak).
3. Update the bib entry: replace citation, finding, relevance.
4. Update any prose that cites the old paper.

### Step 4 — Audit the retained pre-2020 papers

For each pre-2020 paper kept, write a single sentence in `annotated_bib.md` header:

```markdown
## Retained pre-2020 papers (n=4 of 28, 14%)

- Card (1990) — foundational natural-experiment design.
- Imbens & Angrist (1994) — original LATE result.
- Hsieh & Klenow (2009) — canonical misallocation paper that this work
  extends.
- Bertrand, Duflo, Mullainathan (2004) — original clustered-SE-in-DiD warning.
```

This shows the reviewer that pre-2020 inclusion is deliberate, not lazy.

### Step 5 — Re-verify

Compute share again. Confirm ≥80%.

## What success looks like

- `annotated_bib.md` shows ≥80% post-2020 papers.
- Every retained pre-2020 paper has a one-sentence justification at the top.
- Every replaced pre-2020 paper has been confirmed equivalent in scope.

## What failure looks like

- "I replaced Khan-Lu 2013 with a 2024 paper that has a different sample and treatment." The replacement is not equivalent; either keep both (one in main text, one in appendix) or find a better replacement.
- "I kept Khan-Lu 2013 because I cannot find a replacement." Search harder. Use Semantic Scholar's cited-by. If after a 30-minute search you genuinely cannot find a modern equivalent, you have probably found a real gap in the recent literature — note this in `dead_branches.md` as a future paper idea.
- "I am over 80% but my mechanism paragraph still cites 2003 papers exclusively." Section-level imbalance matters too. Aim for 80% per section, not just globally.

## Pitfalls

- Replacing a paper without re-reading the prose that cited it. If your intro cited Khan-Lu for the "audit-monitoring channel" and you replace with Chen 2024 (which is about CFO tenure), the prose now misattributes the channel to Chen.
- Treating "2020+" as a hard cutoff with no judgment. A 2019 paper that is on-point is better than a 2024 paper that is off-point. The 80% is a target, not a religion.
- Skipping Phase 7 because Phase 1 already hit 75%. The gap from 75% to 80% is one or two strategic replacements; do them now, not at submission.

## Time budget

Half a day to a full day. Searching for replacements is the slow part; once found, replacement is quick.
