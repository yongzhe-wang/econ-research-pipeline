<p align="center">
  <img src="docs/assets/banner.svg" alt="econ-research-pipeline" width="700">
</p>

<p align="center">
  <strong>FROM RAW DATA TO TOP-JOURNAL DRAFT.</strong>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge" alt="MIT License"></a>
  <a href="https://github.com/yongzhe-wang/econ-research-pipeline/stargazers"><img src="https://img.shields.io/github/stars/yongzhe-wang/econ-research-pipeline?style=for-the-badge" alt="Stars"></a>
  <a href="https://github.com/yongzhe-wang/econ-research-pipeline/issues"><img src="https://img.shields.io/github/issues/yongzhe-wang/econ-research-pipeline?style=for-the-badge" alt="Issues"></a>
  <a href="SYSTEM_PROMPT.md"><img src="https://img.shields.io/badge/System_Prompt-4117_words-purple?style=for-the-badge" alt="System Prompt"></a>
</p>

**econ-research-pipeline** is a 10-phase workflow that takes an empirical economics or accounting researcher from raw data to a top-journal-quality paper draft. Built from a real research project and battle-tested against six recurring failure modes — citation fabrication, mean reversion, selection on switches, bundled-treatment confounds, HonestDiD breakdowns, and search-snippets-as-reading.

[System Prompt](SYSTEM_PROMPT.md) · [Quickstart](QUICKSTART.md) · [Workflow](workflow/) · [Templates](templates/) · [Examples](examples/) · [Failure Modes](docs/failure_modes.md) · [Citation Integrity](docs/citation_integrity.md)

## What this kit gives you

- A complete **4,117-word system prompt** for Claude or GPT to run the entire pipeline end-to-end
- **12 phase guides** covering ingest, literature scan, cross-model red-team, empirical exploration, killer tests, drafting, and polish
- **Top-journal section templates** distilled from TAR, JAR, JAE, RAST exemplars (abstract through conclusion plus tables, figures, bibtex)
- **Citation verification protocol** that consistently catches LLM citation fabrications before they hit a referee
- **Codex CLI cross-model red-team** recipe — second-opinion validation for novelty and identification claims
- **HonestDiD sensitivity testing** baked in as a mandatory robustness step, not an afterthought

## The 12 phases

| # | Phase | What it does |
|---|---|---|
| 0 | Setup | Install Python venv, Tectonic, Codex CLI |
| 1 | Data ingest | Profile schema, flag quality issues, build clean panel |
| 2 | Literature scan | Parallel agents scan facets of the literature |
| 3 | Codex red-team | Cross-model challenge of novelty and identification claims |
| 4 | Empirical exploration | 60+ regression battery; let the data surface signals |
| 5 | Killer tests | Placebo, transition matrix, HonestDiD — kill weak claims |
| 6 | Direction lock | Commit to one paper backed by survived tests |
| 7 | Exemplar collection | Pull top-journal benchmarks for section templates |
| 8 | Lit modernization | 80%+ citations from 2020 onward |
| 9 | Citation verification | Read abstracts and intros; flag misrepresentation |
| 10 | Paper draft | Full LaTeX draft with verified everything |
| 11 | Polish and reframe | Voice pass, honest reframing of weak claims |

## Quickstart

```bash
git clone https://github.com/yongzhe-wang/econ-research-pipeline.git
cd econ-research-pipeline
# Read SYSTEM_PROMPT.md, paste into your assistant as the system prompt.
# Copy templates/paper-folder-template/ to start your first paper.
```

Full guide: see [QUICKSTART.md](QUICKSTART.md).

## Stack

- **Claude Code** by Anthropic — orchestrator and parallel subagents
- **Codex CLI** by OpenAI — cross-model red-team
- **Python** — pandas, statsmodels, linearmodels, pyfixest, scipy, matplotlib
- **R** — HonestDiD package, called from Python via subprocess
- **Tectonic** — modern LaTeX engine (~50 MB), compiles to PDF without a full TeX install
- **Typical data sources** — CSMAR, WRDS, FRED, Compustat (bring your own data layer)

## Acknowledgments

Built during a real research project on Chinese A-share auditor characteristics and corporate tax avoidance. Methodology learned the hard way. If this pipeline shortens your paper timeline, a star on the repo is appreciated.

## License

MIT — see [LICENSE](LICENSE).
