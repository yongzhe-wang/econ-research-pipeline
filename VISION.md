# Vision

## Why this exists

The gap between LLM hype and the actual process of writing an econ paper is wide enough to swallow a dissertation. Marketing copy promises that a model will read the literature, run the regressions, write the draft, and check the citations. The reality is that a researcher who pastes a vague prompt into a chat window gets a confident-sounding survey paragraph with three fabricated citations, a regression specification that ignores serial correlation, and a lit review padded with 2015-era work. The model is willing; the methodology is missing. This pipeline closes that gap by encoding the actual sequence — data, lit, identification stress-test, empirics, killer tests, direction lock, draft, citation audit, polish — as twelve sequential phases with mandatory STOP gates at every place a model alone reliably fails.

## What econ paper writing looks like without it

A typical project starts with a hunch, a CSMAR or WRDS extract, and a Stata script that grows by accretion. The researcher reads a hundred papers in parallel with running regressions, settles on an angle around month three, drafts an introduction whose lit review skews 2010-2018 because that is what they read first, sends the draft to a coauthor at month five, gets told the parallel-trends assumption is undefended and the identification has a hole, restarts at month seven. Two to three such loops per paper is normal. Fabricated citations from an over-helpful model show up at month eight when a referee Googles the cite and finds no such paper exists.

## What this pipeline does

It compresses the loop. Phase 1 profiles the data before any regression. Phase 2 fans five-plus parallel agents across facets of the literature so coverage is not biased by reading order. Phase 3 runs a cross-model red-team (Claude as primary, Codex CLI as challenger) against the novelty and identification claims before a single regression specification gets fixed. Phase 4 sweeps 60+ regressions to map the empirical surface. Phase 5 runs killer tests — placebo, transition matrix, HonestDiD — that try to falsify your headline. Phases 8 and 9 verify every cited paper exists and says what you claim it says. The mistakes that used to surface at referee report stage now surface in week two.

## Where this is going

The current release covers setup through polish. Planned future work: an evaluation suite that scores a candidate paper against the failure-mode catalog before submission; a reproducibility harness that re-runs every regression from raw data with a single command; additional phase prompts for special cases (event study, RDD, IV with weak instruments); a growing failure-mode library; translated phase guides starting with zh-CN; and a small set of Claude Code skills that wrap the most common phase prompts so a user can invoke them by name.

## An honest scope note

This methodology came from one real project on Chinese A-share auditor characteristics and tax avoidance. It does not claim to be universally optimal. The killer-tests battery is tuned to settings where panel data, staggered treatment, and parallel-trends concerns dominate. For a structural IO paper, a labor RDD, or a macro VAR, several phases will need adaptation — the twelve-phase scaffold carries over, the specific tests inside each phase do not. This is also not a paper generator. Every identification claim, every citation, every direction call requires a human to sign off at a STOP gate. The pipeline compresses the timeline; it does not replace the researcher.

## Invitation

Forks for other fields are welcome and explicitly invited. Finance, IO, labor, development, macro — if you adapt the killer-tests battery or the section templates to your subfield, open a PR or link your fork in an issue. New failure modes with detection tests are the highest-value contribution: if you hit a research mistake the pipeline did not catch, document it in `docs/failure_modes.md` style and submit a PR. New section templates from top journals (TAR, JAR, JAE, RAST, QJE, AER, JFE, RFS) are welcome. Translation of phase guides into zh-CN or other languages is explicitly invited. See [CONTRIBUTING.md](CONTRIBUTING.md) for the small print.
