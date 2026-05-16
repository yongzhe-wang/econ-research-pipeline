# econ-research-pipeline

A replication kit for running an econ paper from raw data to a top-journal-quality draft through a 10-phase workflow. Built from a real Chinese A-share research project; battle-tested against six fabricated-citation incidents, two failed identification slices, and one mean-reversion trap that nearly shipped.

## What this kit gives you

- A **system prompt** (`SYSTEM_PROMPT.md`) you paste into Claude Code / Claude.ai project instructions to load the entire methodology — 10 phases, hard rules, voice conventions, failure-mode awareness.
- A **paper folder template** (`templates/paper-folder-template/`) you copy once per paper. Pre-wired skeleton with `goal.md`, `design.md`, `code/`, `data/`, `refs/`, `results/`, `draft/`.
- **Phase prompts** (`templates/phase-prompts/`) — copy-paste prompts that drive each phase, with built-in STOP gates so the human stays in the loop on identification calls.
- A **section kit** (`templates/section_kit/`) — 11 section-level scaffolds for the LaTeX draft (abstract through bibtex style), aligned with top-5 journal conventions.
- **Documented failure modes** (`docs/failure_modes.md`) — six real failures the original project hit, with detection and recovery procedures so you do not hit them again.

## Quickstart

See [`QUICKSTART.md`](QUICKSTART.md) for a 10-minute first-paper walkthrough. The short version:

```bash
git clone <this repo> ~/econ-research-pipeline
cp -r ~/econ-research-pipeline/templates/paper-folder-template ~/my-papers/<slug>/
cd ~/my-papers/<slug>/
# fill goal.md
# load SYSTEM_PROMPT.md into Claude Code as project instructions
# paste phase-prompts/phase1-litreview.md, follow STOP gates
```

## Example output

`examples/ff-loss-tax-avoidance/` contains anonymized artifacts from the originating project: the `goal.md`, `design.md`, and `headline_results.md` showing the locked direction, headline coefficient, HonestDiD breakdown, and mechanism null. The actual paper PDF is private; the workflow artifacts are open.

## Phases at a glance

| Phase | Name | One-line objective |
|-------|------|--------------------|
| 0 | Data ingest | Profile and clean before anyone asks "what's the paper". |
| 1 | Literature scan | Find 20-30 papers, identify the gap, propose 3 angles. |
| 2 | Codex red-team | Cross-model check: any competitor papers? Any fatal ID flaw? |
| 3 | Empirical exploration | Run 60+ regressions to map the empirical landscape. |
| 4 | Killer tests | Placebo, transition matrix, asymmetric framing, HonestDiD. |
| 5 | Direction lock | Commit to a headline. Document what was killed. |
| 6 | Exemplar collection | Build a corpus of 5-10 top-journal models for each section. |
| 7 | Lit modernization | Push 80%+ of citations to 2020+. |
| 8 | Citation verification | Read every paper. Tag verified. Catch fabrications. |
| 9 | Paper draft | First full LaTeX compile. Every number traceable. |
| 10 | Polish and reframe | Honest language. Cut overclaims. Final compile. |

## Stack

- **Python**: pandas, statsmodels, linearmodels, pyfixest, scipy, matplotlib
- **R via subprocess**: HonestDiD (Rambachan-Roth) for parallel-trends sensitivity
- **LaTeX**: Tectonic (modern, self-contained, ~50MB; auto-fetches packages)
- **Codex CLI**: GPT-5.5 for cross-model red-team (calibration channel C)
- **Claude Code**: orchestration, parallel subagents, blind-review channel

Install steps in [`docs/tool_setup.md`](docs/tool_setup.md).

## License

MIT. See `LICENSE`.

## Citation

If this pipeline helps your research, please cite:

```
yongzhe-wang. 2026. "econ-research-pipeline: A 10-phase methodology
for econ papers from data to draft." GitHub repository.
https://github.com/yongzhe-wang/econ-research-pipeline
```

## Acknowledgments

The pipeline was developed during a real research project on Chinese A-share auditor characteristics and tax avoidance. The actual paper artifact is private; the methodology that produced it is open-source for the community. Cross-model red-team integration with OpenAI Codex CLI was inspired by 3-channel calibration practice. HonestDiD integration follows Rambachan and Roth (2023, ReStud).

Methodology design and the eight failure modes documented in `docs/failure_modes.md` came from real failure events during that project. Anyone running this pipeline will likely hit new failure modes; pull requests to extend the documentation are welcome.
