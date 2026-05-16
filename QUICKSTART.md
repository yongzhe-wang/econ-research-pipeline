# Quickstart — Your First Paper in 10 Minutes

This guide gets you from `git clone` to the first STOP gate (after Phase 1 lit review). It assumes you have data and a research question, even a rough one.

## Prerequisites

- macOS or Linux. Windows via WSL.
- Python 3.10+ in a virtualenv or conda env.
- Homebrew (macOS) or apt (Linux) for Tectonic.
- Claude Code installed (or access to claude.ai with project instructions).
- (Optional but recommended) Codex CLI installed and authenticated.

## Step 1 — Clone the kit

```bash
git clone https://github.com/yongzhe-wang/econ-research-pipeline.git ~/econ-research-pipeline
cd ~/econ-research-pipeline
```

## Step 2 — Set up Python environment

```bash
python -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install pandas numpy statsmodels linearmodels pyfixest scipy matplotlib \
            seaborn binsreg rdrobust doubleml econml stargazer pyarrow
```

If you plan to run HonestDiD (recommended — it is the Phase 4 binding test for staggered DiD), also install R and the `HonestDiD` package:

```bash
# macOS
brew install r
R -e 'install.packages("HonestDiD", repos="https://cloud.r-project.org")'
```

## Step 3 — Install Tectonic

```bash
# macOS
brew install tectonic

# Linux
curl --proto '=https' --tlsv1.2 -fsSL https://drop-sh.fullyjustified.net | sh
```

Verify: `tectonic --version`.

## Step 4 — (Optional) Install and authenticate Codex CLI

```bash
npm install -g @openai/codex
codex auth login
```

Without Codex, the Phase 2 cross-model red-team falls back to manual review or to another available model. Codex (GPT-5.5) is the reference; any other capable model works as channel C.

## Step 5 — Create your paper folder

Pick a short slug (lowercase, hyphens). Example: `minimum-wage-employment`.

```bash
mkdir -p ~/my-papers
cp -r ~/econ-research-pipeline/templates/paper-folder-template ~/my-papers/minimum-wage-employment
cd ~/my-papers/minimum-wage-employment
```

You should see:

```
goal.md          design.md        paper.md      annotated_bib.md
code/            data/            refs/         results/        draft/
```

## Step 6 — Drop your data into `data/`

The repo-root `data/` directory is for raw, canonical files. The paper-local `data/` is for intermediates only. For your first paper, just put the raw file in the paper-local `data/` and adjust paths later if you grow into multiple papers sharing data.

```bash
cp ~/path/to/your_raw_data.csv data/
```

## Step 7 — Fill `goal.md`

Open `goal.md` in your editor. Fill all sections. Be concrete. Example structure:

```markdown
# Goal — Minimum Wage and Employment in US Counties

## Research question (one sentence)
Does the 2024 federal minimum wage increase reduce low-wage employment in
counties whose pre-policy wage floor was below the new federal level?

## What I want to know (3 bullets, plain English)
- By how much does employment fall in bound counties relative to unbound?
- Is the effect concentrated in restaurants/retail or broad-based?
- Does the effect persist 12 months out or fade?

## Data I have
- Source: BLS QCEW, county-quarter
- Unit of observation: county-quarter
- Time coverage: 2018Q1 to 2025Q2
- Sample size: ~3,140 counties x 30 quarters = ~94k obs
- Key variables: employment, total wages, NAICS sector, county FIPS

## Why this matters
[1-2 sentences]

## Hypotheses (pre-specified)
- H1 (main): bound counties show employment decline at t=0 to t=+2.
- H2 (sector): effect concentrates in NAICS 72 (food services).
- H3 (placebo): unbound counties show no effect.
```

The pre-specified hypotheses are load-bearing. The pipeline will hold you to them at Phase 4 (killer tests). If your data does not support H1 but does support a different framing, that framing requires its own pre-spec and an honest disclosure that you updated.

## Step 8 — Load the system prompt into Claude Code

In Claude Code, set the project instructions / system prompt by pasting the contents of `~/econ-research-pipeline/SYSTEM_PROMPT.md`. In `claude.ai`, create a Project and paste it into the project instructions.

The system prompt loads the entire methodology: hard rules, phase workflow, voice conventions, failure-mode awareness. Once loaded, the assistant will know to start with Phase 0 (data profiling) before anything else.

## Step 9 — Start Phase 0 (data profiling)

In Claude with the system prompt loaded, say:

```
My paper folder is ~/my-papers/minimum-wage-employment. Run Phase 0.
```

The assistant will load the data, print shape and head, profile every column, flag quality issues, define the analytic sample, save `data/<slug>_clean.parquet`, and write `results/data_quality_report.md`. It will stop and ask you to approve the analytic sample before moving on.

## Step 10 — Start Phase 1 (literature scan)

Once Phase 0 is approved, paste:

```
Run Phase 1.
```

The assistant will spawn 5 parallel subagents, search NBER + SSRN + top-5 + field journals + Chinese sources, return 20-30 papers, write `annotated_bib.md`, and propose 3 candidate contribution angles. It will stop at the STOP gate and ask you to pick an angle before Phase 2.

You can also paste `templates/phase-prompts/phase1-litreview.md` directly if you prefer the verbose form.

## What comes next

After Phase 1 approval, run Phases 2 through 10 in order. Each phase has a STOP gate. Total wall-clock for a first paper: roughly 4-8 weeks elapsed, with 2-3 active hours per day.

The full phase-by-phase guide is in `workflow/`:
- `workflow/00_setup.md` through `workflow/11_polish_reframe.md`.

Refer to `docs/` for deeper dives:
- `docs/failure_modes.md` — six real failures with detection.
- `docs/citation_integrity.md` — fabrication protocol.
- `docs/tool_setup.md` — install troubleshooting.
- `docs/codex_integration.md` — cross-model recipe.

## Common first-day issues

- **"Phase 0 says the analytic sample is N=12,400 but my raw file has 30,000 rows."** That is normal. The Phase 0 report tells you exactly which filters dropped how many rows. Read it. If a filter is wrong, push back and the assistant will re-run.
- **"The assistant skipped Phase 0 and went straight to regressions."** Reload the system prompt. Phase 0 is a hard rule.
- **"Codex flagged 3 competitor papers I never heard of."** Read them. If they cover your angle, you need a new angle or a sharper differentiation. This is Phase 2 working as intended.
- **"Tectonic compile fails on first try."** Tectonic auto-fetches packages on first compile. Run it twice. If it still fails, check `docs/tool_setup.md`.
