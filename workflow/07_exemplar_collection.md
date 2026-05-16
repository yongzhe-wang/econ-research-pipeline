# Phase 6 — Top-Journal Exemplar Collection

## Objective

Build a corpus of 5-10 top-journal exemplar papers in the same identification family as your paper. Extract their section templates. Annotate the section kit with which exemplar each subsection should mirror.

## Inputs

- Locked direction from Phase 5.
- `annotated_bib.md`.

## Outputs

- `refs/exemplars/` containing PDF + extracted text for each exemplar.
- Annotations on `templates/section_kit/` files indicating which exemplar to model each section on.
- `results/exemplar_notes.md` — what each exemplar does well, what to copy.

## Why exemplars

Top-journal papers in the same ID family have already solved the writing problems you face. The intro structure, the lit-review framing, the data-section level of detail, the empirical-strategy presentation — all conventional. You do not need to invent them.

Pick exemplars from the same identification family. A staggered DiD paper is not a good template for an RDD paper. A 2024 within-firm event study is a good template for a 2026 within-firm event study.

## Selection criteria

1. **Same identification family.** Within-firm event study? Find within-firm event-study exemplars. Staggered DiD? Find staggered DiD exemplars. RDD? Find RDD exemplars. IV? Find IV exemplars.
2. **2022-2026 publication date.** Older papers use older conventions (e.g., classic 2WFE without disclosure of staggered-DiD bias). Modern papers integrate Callaway-Sant'Anna, Sun-Abraham, HonestDiD as standard. Mirror modern conventions.
3. **Top-5 or top field journal.** AER, QJE, JPE, ReStud, Econometrica for top-5. JAE, JAR, TAR, JFE, RFS, JF for top field. The level of detail and the standard of robustness differ markedly from second-tier journals.
4. **Same data scale / type.** Firm-year panels are different from individual-level cross-sections. Pick exemplars whose data scale matches yours.
5. **Recent citations.** Cited frequently in the last 2 years (signals influence).

Pick 5-10. More is diminishing returns; fewer means you do not have a model for every section.

## Acquisition

For each exemplar:

```bash
# Download PDF (publisher page or NBER working paper version)
curl -O https://www.aeaweb.org/articles?id=10.1257/aer.20xxxxxx

# Extract text
pdftotext exemplar.pdf refs/exemplars/<key>.txt
```

If the PDF is paywalled, use the NBER working paper version (often identical content, different formatting). If still inaccessible, request via institutional access or skip and replace.

## Section-by-section annotation

Open `templates/section_kit/` and annotate each section file with the exemplar to model. Example annotations:

`01_INTRO.md`:
```
Model on Smith2024_AER intro (5-paragraph structure):
- Paragraph 1: 3-sentence hook ending with the headline number.
- Paragraph 2: gap statement (what literature does not answer, with 2 citations).
- Paragraph 3: data + identification one-paragraph summary.
- Paragraph 4: headline result + binding falsification.
- Paragraph 5: contribution to literature (2-3 distinct contributions).
```

`05_RESULTS.md`:
```
Model on Jones2025_QJE results section:
- Subsection 5.1: main table, FE rows labeled explicitly.
- Subsection 5.2: event-study plot, with M-bar curve in appendix.
- Subsection 5.3: heterogeneity by 1-2 dimensions only (do not bury the headline).
```

## Capture in exemplar_notes.md

For each exemplar, write 3-5 bullet points:

```markdown
## Smith et al. 2024 (AER) — Within-firm event study of X on Y

- Intro structure: 5 paragraphs, headline in paragraph 1. Mirror.
- Data section: 1.5 pages, with a "construction of sample" subsection. Mirror.
- Robustness: 8 checks, with the headline 4 in main table and 4 in appendix. Mirror.
- HonestDiD: integrated into main text, not in appendix. Mirror.
- Mechanism: half-page; restrained interpretation. Mirror tone.

## Jones et al. 2025 (QJE) — Staggered DiD with Callaway-Sant'Anna

- Empirical strategy section: 2 pages, explicitly justifies CS over 2WFE. Mirror the justification.
- Event-study plot: M-bar sensitivity curve in same figure. Mirror.
- Heterogeneity: only 2 dimensions, sharp framing. Mirror.
```

## What success looks like

- 5-10 exemplars in `refs/exemplars/` with both PDF and extracted text.
- Every section in `section_kit/` has at least one named exemplar.
- `exemplar_notes.md` captures what to copy from each exemplar.

## What failure looks like

- "I picked 3 exemplars from 2010-2015 because they are foundational." Foundational papers can be cited; they should not be your section template. Conventions have evolved.
- "I picked exemplars from a different identification family." The whole point is matching identification. Replace.
- "I extracted PDFs but did not annotate." The annotations are the deliverable.

## Pitfalls

- Treating exemplars as content sources. They are template sources. Do not paste their sentences into your paper; do paste their section structure.
- Picking exemplars by famous-author name rather than by section-quality match. The most-cited paper in the field may have a clunky data section; pick a less-cited paper with a better-written data section.
- Skipping this phase because "I already know how to write a paper." Maybe. But for any non-trivial paper, the section kit + exemplars give you a 30-50% speedup in drafting and reduce the revision count.

## Time budget

1-2 days. Acquisition and text extraction is fast; the value is in the reading and annotation, which takes time.
