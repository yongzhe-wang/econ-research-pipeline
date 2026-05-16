# Paper folder template

Copy this folder once per paper, under whichever project it belongs to:

```bash
cp -r ~/Desktop/econ_test/templates/paper-folder-template ~/Desktop/econ_test/projectN/papers/<slug>/
cd ~/Desktop/econ_test/projectN/papers/<slug>/
```

(Replace `projectN` with `project1`, `project2`, etc.)

Then:

1. Edit `goal.md` — the research question and what data you have.
2. Raw data does NOT go here. Raw, canonical datasets live at the repo-root `data/` directory (`~/Desktop/econ_test/data/`) and are referenced from this paper's code as `../../../data/<file>`. The paper-level `data/` directory below is for *intermediates only* — cleaned subsamples, merged tables, residualized files produced by your own code.
3. Drop PDFs and `.bib` into `refs/`.
4. Open Claude. Paste `~/Desktop/econ_test/templates/phase-prompts/phase1-litreview.md`, prefixed with `My paper folder is ~/Desktop/econ_test/projectN/papers/<slug>/`.
5. Approve at each STOP gate. Run phase prompts in order: 1 → 2 → 3 → 4 → 5.
6. Track progress in `paper.md`.

## Layout

```
<slug>/
├── README.md              this file
├── paper.md               state tracker — check off phases as you go
├── goal.md                fill in BEFORE Phase 1
├── data/                  paper-local INTERMEDIATES only (raw lives at root /data)
├── refs/                  PDFs and supplementary .bib files
├── annotated_bib.md       Phase 1 output
├── design.md              Phase 2 output
├── code/                  Phase 3 output (01_clean.py ... 05_heterogeneity.py)
├── results/               tables, figures, run_log.txt, interpretation.md
└── draft/
    ├── paper.tex          Phase 4 output
    └── refs.bib           BibTeX (populated through Phases 1 & 5)
```

## Don't

- Don't edit `annotated_bib.md`, `design.md`, or `code/*.py` directly before the relevant phase runs — let the RA fill them per the phase prompt, then edit after.
- Don't skip STOP gates. The whole point of the pipeline is that you stay in the loop on identification and interpretation.
