# Tool Setup

## Python environment

```bash
python -m venv .venv
source .venv/bin/activate
pip install -U pip

pip install \
  pandas numpy scipy \
  statsmodels linearmodels pyfixest \
  matplotlib seaborn \
  binsreg rdrobust doubleml econml \
  stargazer \
  pyarrow jupyter ipykernel \
  requests beautifulsoup4
```

`requirements.txt`:

```
pandas>=2.0
numpy>=1.24
scipy>=1.11
statsmodels>=0.14
linearmodels>=5.3
pyfixest>=0.20
matplotlib>=3.7
seaborn>=0.13
binsreg
rdrobust
doubleml>=0.7
econml>=0.14
stargazer
pyarrow
jupyter
ipykernel
requests
beautifulsoup4
```

### Troubleshooting

- **`pip install pyfixest` errors on Apple Silicon.** Try `pip install --pre pyfixest` or `conda install -c conda-forge pyfixest`. The package depends on `numpy` with native BLAS; the conda-forge build is more reliable on M-series Macs.
- **`pip install rdrobust` errors with C++ compilation.** Install Xcode command-line tools: `xcode-select --install`.
- **`pip install econml` is slow.** Normal; pulls many ML dependencies. Allow 5-10 minutes.

## Tectonic (LaTeX)

Tectonic is a modern self-contained LaTeX engine. ~50MB install. Auto-fetches CTAN packages on first compile. No 4GB MacTeX needed.

### Install

```bash
# macOS
brew install tectonic

# Linux (curl installer)
curl --proto '=https' --tlsv1.2 -fsSL https://drop-sh.fullyjustified.net | sh

# Or via Cargo (any platform)
cargo install tectonic
```

### Verify

```bash
mkdir -p /tmp/tec-test && cd /tmp/tec-test
cat > hello.tex <<'EOF'
\documentclass{article}
\usepackage{booktabs}
\usepackage{natbib}
\begin{document}
Hello, world. See \citet{smith2024}.
\bibliographystyle{chicago}
\end{document}
EOF
tectonic hello.tex
ls hello.pdf
```

First run downloads ~150MB of packages. Be patient.

### Common compile issues

- **"Unable to find resource"** — Tectonic's package cache is stale. Run `tectonic --reruns 3 paper.tex` or delete `~/.cache/Tectonic/` and retry.
- **Missing citation warnings** — your `\cite{key}` does not match an entry in `refs.bib`. Check the bib entry key.
- **Hangs on first compile** — network slow or blocked. Tectonic pulls from CTAN; ensure outbound HTTPS works.

## R (for HonestDiD only)

The HonestDiD package (Rambachan-Roth parallel-trends sensitivity) is R-only. Use R via subprocess; do not embed via rpy2 (subprocess is simpler and reproducible).

### Install

```bash
# macOS
brew install r

# Linux
sudo apt-get install r-base

# Install packages
R -e 'install.packages(c("HonestDiD", "fixest", "data.table"), repos="https://cloud.r-project.org")'
```

### Verify

```bash
Rscript -e 'library(HonestDiD); cat("ok\n")'
```

### Calling from Python

```python
import subprocess
result = subprocess.run(
    ["Rscript", "code/04_honestdid.R", "results/event_study_coefs.csv"],
    check=True,
    capture_output=True,
    text=True,
)
print(result.stdout)
```

## Codex CLI

GPT-5.5 via Codex CLI. Used as calibration channel C.

### Install

```bash
npm install -g @openai/codex
```

Requires Node.js 18+. If you do not have Node:

```bash
# macOS
brew install node

# Linux (Ubuntu/Debian)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### Auth

```bash
codex auth login
# follow browser flow; needs an OpenAI API key
```

### Verify

```bash
echo "What is 2+2?" | codex exec
```

### Usage patterns

One-shot exec mode (recommended for pipeline use):

```bash
codex exec <<'EOF'
You are a red-team reviewer. ...
[your full prompt with all context inlined]
EOF
```

Interactive mode (for exploration):

```bash
codex
# opens REPL
```

The pipeline uses one-shot mode at Phases 2, 4 (optional double-check), and 10. See `docs/codex_integration.md` for the exact prompts.

## PDF reading (pdftotext)

Used for extracting text from exemplar PDFs in Phase 6.

```bash
brew install poppler

# Verify
pdftotext --help
```

Usage:

```bash
pdftotext exemplar.pdf exemplar.txt
```

## Optional but useful

### git

```bash
brew install git
```

Pipeline assumes you commit your paper folder frequently. `~/my-papers/<slug>/` is a git repo; commit after each phase.

### gh (GitHub CLI)

```bash
brew install gh
gh auth login
```

For pushing the paper folder to a private repo or for opening PRs on the pipeline kit itself.

### TeX-aware editor

VSCode + LaTeX Workshop extension. Or Cursor. Or Overleaf if you prefer a hosted IDE (compile with Tectonic in the build chain).

## Verification — full round trip

After all tools are installed:

```bash
cd ~/my-papers/<slug>

# Python OK
python -c "import pyfixest, statsmodels, linearmodels; print('python OK')"

# R OK
Rscript -e 'library(HonestDiD); cat("R OK\n")'

# Tectonic OK
echo '\documentclass{article}\begin{document}ok\end{document}' > /tmp/t.tex
tectonic /tmp/t.tex && echo "tectonic OK"

# Codex OK (if installed)
echo "OK" | codex exec | head -1
```

If all four print OK, you are ready to run the pipeline.

## Disk footprint

Total for the full kit:
- Python venv: ~2-3 GB (pandas + scipy + matplotlib + econml drag in many deps).
- Tectonic + cached packages: ~200 MB after first compile.
- R + HonestDiD + deps: ~500 MB.
- Codex CLI: ~50 MB.

Plan for ~4 GB on disk for the toolchain. Data files are separate.
