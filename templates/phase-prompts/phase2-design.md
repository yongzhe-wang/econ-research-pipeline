# Phase 2 — Research Design

You are my econ RA. I picked **angle X** from `annotated_bib.md` (I'll tell you which above). The paper folder is `<PAPER_FOLDER_PATH>`.

Read in order:
1. `goal.md`
2. `annotated_bib.md` (especially the gap analysis and my chosen angle)
3. `data/` — profile every file: column names, dtypes, sample size, key descriptive stats (`df.describe()` mentally), missingness, unit of observation, time coverage. Use `head` and `wc -l` from bash.
4. `~/Desktop/econ_test/style.md` and `~/Desktop/econ_test/econ-toolbox.md`.

Output `design.md` with the following sections, in this order:

### Research question (sharpened)
One sentence. Specific enough that the empirical answer is yes/no/a-number.

### Conceptual framework
2-3 paragraphs. The economic mechanism you're testing. Cite the theoretical anchors from `annotated_bib.md`. Keep it tight — this is not a model section, it's the logic of why the data should answer the question.

### Identifying variation
What generates exogeneity? Be specific: a policy reform, a discontinuity, a natural experiment, a panel of within-unit variation, an instrument. Name it.

### Main estimating equation
LaTeX block. Define every symbol below it. Specify FE, cluster level, sample, functional form.

### Sample definition
- Universe: all <units> in <data source>
- Filters: list each inclusion/exclusion rule + the rationale + how many obs each rule drops
- Final N (estimate)

### Variables
Table: variable name (paper) | data column (raw) | construction | role (outcome / treatment / control / FE / cluster).

### Identification threats with mitigations
Bullet list. For each threat, name it (selection, reverse causality, omitted variable, measurement error, SUTVA violation, anticipation, etc.) and the mitigation (control, alternative spec, placebo, sensitivity bound).

### Robustness checks queue
List 5-8 robustness checks to run in Phase 3: alternative specs, alternative samples, placebos, leave-one-out, alternative clustering, alternative functional form.

### Pre-specified hypotheses
For each main coefficient, state: predicted sign, predicted approximate magnitude (if theory gives one), and what would falsify the mechanism.

**STOP here for me to approve before any code is written.**
