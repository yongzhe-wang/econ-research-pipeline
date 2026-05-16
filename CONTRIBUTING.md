# Contributing

Thanks for considering a contribution. This pipeline gets stronger with every new failure mode caught and every new section template added, so even small PRs are welcome.

## How to propose changes

- **Small fix (typo, broken link, clarification)** — open a PR directly.
- **New failure mode, new template, new phase prompt, anything affecting the methodology** — open an issue first describing what you want to add and why. We will talk it through before you spend time writing.
- **Big refactor of an existing phase guide** — issue first. The phase guides are load-bearing; changes need to be deliberate.

## What kinds of contributions are welcome

- **New failure modes with detection tests.** Highest value. If you hit a research mistake the pipeline did not catch, document it in `docs/failure_modes.md` style: what the false signal looked like, how you detected it, how to recover. Include a minimal test that flags the same pattern.
- **New section templates from top journals.** TAR / JAR / JAE / RAST / QJE / AER / JFE / RFS. Pull a published exemplar, distill the structural moves into `templates/section_kit/`, cite the source paper in the file header.
- **Phase prompt refinements.** If a phase prompt in `templates/phase-prompts/` produced a weak result for you and you found a prompt that worked better, open a PR with both versions and the reasoning.
- **Bug reports in workflow scripts.** Anything in `workflow/` or `templates/` that breaks on a fresh install — file a bug report (template under `.github/ISSUE_TEMPLATE/`).
- **Translation.** Phase guides translated into zh-CN are welcome. Chinese-English mixed thinking docs (the way the original methodology was actually written) are fine to submit in zh-CN; we will not force monolingual English.
- **Forks for other fields.** Finance, IO, labor, macro — if you adapt the killer-tests battery or the section templates, link your fork in an issue and we will reference it from `VISION.md`.

## PR checklist

Before opening a PR, confirm:

- [ ] **Cite real papers only.** Every citation in any file you touch has been verified on the publisher page or on Google Scholar. No model-fabricated citations. If you cannot find the paper, do not cite it.
- [ ] **Respect the 2020+ rule in lit examples.** If your PR touches `templates/section_kit/` or example bibliographies, at least 80% of cited works should be from 2020 or later. This matches phase 8 (lit modernization).
- [ ] **If adding a failure mode, include a detection test.** A failure mode without a test is just a war story. The test can be a regex pattern, a regression spec, a checklist — but something concrete a future user can run.
- [ ] **Style: terse, active, no LLM tells.** No "in this section, we will discuss", no "delving into", no "comprehensive overview". Read `templates/style.md` first if unsure.
- [ ] **No secrets.** No real data files, no API keys, no proprietary CSMAR / WRDS extracts.

## Local development setup

See [docs/tool_setup.md](docs/tool_setup.md). Short version: a Python venv with `pandas`, `statsmodels`, `linearmodels`, `pyfixest`, `scipy`, `matplotlib`; R with the `HonestDiD` package; Tectonic for LaTeX; Codex CLI for the channel-C red-team. None of this is needed to contribute documentation or prompt changes — only if you are extending the workflow scripts.

## Code of conduct

This project follows the [Contributor Covenant v2.1](CODE_OF_CONDUCT.md). Be civil. Critique work, not people.

## License

By contributing, you agree your contribution is released under the same [MIT license](LICENSE) as the rest of the project.
