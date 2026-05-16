# Phase 0 prerequisites — Setup

## Objective

Stand up the folder structure, install tools, load the system prompt, verify the environment can run a regression and compile LaTeX. Catch tool problems on day zero rather than at Phase 4 when a HonestDiD subprocess silently fails.

## Inputs

- A research idea, even rough.
- Raw data file(s), in any format pandas can read (csv, parquet, dta, xlsx).

## Outputs

- `~/my-papers/<slug>/` paper folder created from the template.
- `~/my-papers/<slug>/.venv/` Python environment with all required packages.
- A successful round-trip test: load data, run an OLS, compile a one-page LaTeX scratch document.
- System prompt loaded into Claude Code (or claude.ai project).

## Steps

### 1. Clone the kit

```bash
git clone https://github.com/yongzhe-wang/econ-research-pipeline.git ~/econ-research-pipeline
```

### 2. Create the paper folder

```bash
mkdir -p ~/my-papers
cp -r ~/econ-research-pipeline/templates/paper-folder-template ~/my-papers/<slug>
cd ~/my-papers/<slug>
```

### 3. Python environment

```bash
python -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install pandas numpy statsmodels linearmodels pyfixest scipy matplotlib \
            seaborn binsreg rdrobust doubleml econml stargazer pyarrow \
            jupyter ipykernel
```

Test:

```bash
python -c "import pandas, statsmodels, linearmodels, pyfixest; print('ok')"
```

### 4. Tectonic (LaTeX)

```bash
brew install tectonic    # macOS
# or:
curl --proto '=https' --tlsv1.2 -fsSL https://drop-sh.fullyjustified.net | sh
```

Test:

```bash
mkdir -p /tmp/tectonic-test && cd /tmp/tectonic-test
cat > hello.tex <<'EOF'
\documentclass{article}
\usepackage{booktabs}
\begin{document}
Hello.
\end{document}
EOF
tectonic hello.tex
test -f hello.pdf && echo "OK" || echo "FAIL"
```

First run downloads ~150MB of packages. Be patient.

### 5. R (for HonestDiD only)

```bash
brew install r
R -e 'install.packages(c("HonestDiD", "fixest"), repos="https://cloud.r-project.org")'
```

Test:

```bash
Rscript -e 'library(HonestDiD); cat("ok\n")'
```

### 6. Codex CLI (optional but recommended)

```bash
npm install -g @openai/codex
codex auth login
```

Test:

```bash
echo "What is 2+2?" | codex exec
```

### 7. Drop raw data

Put the raw data file in `~/my-papers/<slug>/data/`. For very large datasets shared across multiple papers, set up a root-level data directory (e.g., `~/my-papers/data/`) and reference from each paper folder as `../data/`.

### 8. Fill goal.md

Open `goal.md` and fill every section. RQ, what you want to know (3 bullets), data inventory, why it matters, pre-specified hypotheses.

The hypotheses are pre-specified BEFORE Phase 1 lit review. If you wait to write hypotheses until after lit review, you will subconsciously align them with what the literature predicts. Pre-spec is honest signal.

### 9. Load the system prompt

In Claude Code: open the project, set project instructions to the contents of `~/econ-research-pipeline/SYSTEM_PROMPT.md`.

In claude.ai: create a Project, paste into "Custom Instructions".

### 10. Round-trip test

Open Claude with the system prompt loaded. Paste:

```
My paper folder is ~/my-papers/<slug>. Run a smoke test:
load the data, print shape and head, run a one-line OLS of any
continuous outcome on any continuous regressor, and write the
output to results/smoke_test.txt.
```

If the assistant does this without errors, you are ready to start Phase 0 (data profiling).

## What success looks like

- `~/my-papers/<slug>/` exists with the template structure.
- `python -c "import pyfixest"` works.
- `tectonic hello.tex` produces `hello.pdf`.
- `Rscript -e 'library(HonestDiD)'` works.
- `codex exec` returns a sensible reply (optional).
- `results/smoke_test.txt` exists after the round-trip test.

## What failure looks like

- `pip install pyfixest` errors out on Apple Silicon — usually a numpy / blas mismatch. Fix: `pip install --pre pyfixest` or use `conda install -c conda-forge pyfixest`.
- `tectonic hello.tex` hangs forever — your network is slow or blocked. Tectonic pulls from CTAN; ensure outbound HTTPS works.
- `R -e 'install.packages("HonestDiD")'` errors on `RcppArmadillo` — you need a Fortran compiler. `brew install gcc` then retry.
- Codex auth fails — check OpenAI API key. Fall back to using Claude itself for cross-model review (less ideal because same model family, but better than nothing).

## Pitfalls

- Skipping the round-trip test. Tool problems compound; catching them on day zero saves three days at Phase 4.
- Filling `goal.md` after the lit review. Then your hypotheses are post-hoc by definition.
- Using the system Python instead of a venv. Pin to `.venv/` per paper; you will eventually want different package versions for different papers.

## Time budget

30-90 minutes the first time you set up the kit. 5-10 minutes per subsequent paper (just step 2, step 7, step 8, step 9).
