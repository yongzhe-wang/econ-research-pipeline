# Phase 0 — Data Ingest, Profiling, Quality Flagging

## Objective

Know what is in your data before anyone asks what the paper is about. Output a clean analytic sample and a written quality report that every later phase can rely on.

## Inputs

- Raw data file(s) in `data/`.
- `goal.md` with the variable list and intended sample.

## Outputs

- `code/00_profile.py` — the profiling script (reproducible).
- `data/<slug>_clean.parquet` — the cleaned analytic sample.
- `results/data_quality_report.md` — a one-page summary.

## Steps

### Step 1 — Initial load and inspection

```python
import pandas as pd
df = pd.read_csv("data/raw.csv")  # adjust for actual file type
print(df.shape, df.dtypes)
print(df.head())
print(df.tail())
print(df.describe(include="all"))
```

Goal: catch obvious problems (wrong delimiter, encoding issues, dtype mismatches) before downstream code crashes mysteriously.

### Step 2 — Column profiling

For every column, record:
- Missing rate (`df[col].isna().mean()`).
- For continuous: 5-number summary + skew + kurtosis.
- For categorical: top-10 value frequency, total distinct count.
- For text columns: average length, sample values.

Output a tabular profile (one row per column) to `results/column_profile.csv`.

### Step 3 — Constant and near-constant columns

```python
const_cols = [c for c in df.columns if df[c].nunique() == 1]
near_const = [c for c in df.columns if df[c].value_counts(normalize=True).iloc[0] > 0.99]
```

Drop the constant columns. Flag near-constant (>99% same value) for review — sometimes they encode a real binary that happens to be very imbalanced.

### Step 4 — Reconstruct derived variables

Vendor-supplied columns are often wrong or scaled differently. Reconstruct from primitives:
- Leverage = total liabilities / total assets (compare against vendor leverage; if disagreement is >1pp, use reconstructed).
- ROA = net income / total assets.
- BTD = (pre-tax income - tax expense / tax rate) / lag total assets.
- Tobin's Q = (market value of equity + total liabilities) / total assets.

Document every reconstruction in the script with a comment giving the formula and the agreement rate.

### Step 5 — Define the analytic sample with explicit filters

```python
n0 = len(df)
df = df.dropna(subset=["BTD", "CETR", "size", "leverage", "ROA"])
print(f"Drop missing core covariates: {n0 - len(df)} rows")

n1 = len(df)
df = df[df["industry"] != "J"]  # drop financial sector
print(f"Drop financial sector: {n1 - len(df)} rows")

n2 = len(df)
df = df[df["year"] != 2025]  # drop stub year
print(f"Drop year 2025: {n2 - len(df)} rows")
```

Every filter line states a reason and prints the row count dropped. Save the filter log to `results/sample_construction.log`.

### Step 6 — Validate identifiers

Confirm panel structure: each (unit, time) is unique.

```python
dup = df.duplicated(subset=["firm_id", "year"]).sum()
assert dup == 0, f"{dup} duplicates on (firm_id, year)"
```

If duplicates exist, decide deliberately: take first, take last, average, drop. Do not silently allow.

### Step 7 — Save clean sample

```python
df.to_parquet("data/<slug>_clean.parquet", index=False)
```

Parquet is preferred over CSV: typed columns, faster I/O, smaller files.

### Step 8 — Write the quality report

`results/data_quality_report.md` is a one-page summary with:
- Final N, number of unique units, year coverage.
- Filter log (row counts dropped per filter).
- Any reconstructed variables, with agreement rate against vendor version.
- Any flagged quality issues (near-constant columns, large missingness in non-core columns, suspicious value distributions).
- Any decisions made (e.g., "took the last observation when a firm-year had duplicates due to mid-year reorganization").

## What success looks like

You can answer in one sentence: "the analytic sample is N firm-years, J firms, covering years T to T+k, after dropping X for reason A, Y for reason B."

`data/<slug>_clean.parquet` loads cleanly with `pd.read_parquet`. The quality report fits on one page.

## What failure looks like

- "I dropped about 5,000 rows for missingness." Vague — Phase 4 placebo tests need exact counts.
- The clean parquet has columns you do not understand. Go back, profile harder.
- Two filters drop overlapping rows but the log treats them as independent. Use `n1 - len(df)` after each filter, not pre-computed counts.
- The vendor's Big-4 indicator and audit-fee distribution disagree (some "Big-4" firms have audit fees in the bottom decile). Investigate. Often the indicator is miscoded.

## Pitfalls

- Trusting vendor flags. Always validate against the underlying number.
- Skipping the "drop financial sector" step. Financial firms have different accounting; pooling them with non-financials biases everything.
- Failing to handle stub years (the last partial year in a panel often has 1-2 observations per firm and breaks event studies).
- Logging row counts after the fact. Log inline — `n_before = len(df); df = df[mask]; print(n_before - len(df))`.

## Time budget

4-8 hours for a new dataset. 1-2 hours if you have profiled the same vendor before and the new file is just a year extension.
