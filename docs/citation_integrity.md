# Citation Integrity — The Fabrication Problem and Verification Protocol

## The problem

LLM agents fabricate citations. Not occasionally — systematically. The originating project caught at least six fabrications across multiple rounds. The patterns repeat:

- A real author paired with a non-existent paper.
- A real paper attributed to the wrong year (with a slightly different finding line invented to match).
- A real journal name with a non-existent volume / pages.
- A plausible-sounding paper whose DOI does not resolve and which appears in no search index.

These are not occasional mistakes. They are a consistent failure mode that emerges from how LLMs synthesize plausible-sounding combinations from their training data. The author names are real because the agent has seen them. The journal name is real because the agent has seen it. The combination is invented.

## Why this matters

A fabricated citation in a published paper is a career-affecting event. Reviewers spot-check. Discussants spot-check. Replicators spot-check. The protocol below is mandatory precisely because the cost of one undetected fabrication is high.

A real example from the originating project: an LLM agent cited "Aobdia and Lin 2024" for an audit-monitoring channel. The bib entry had a plausible journal, year, page range, and finding line. The DOI did not resolve. No Crossref hit. No Google Scholar hit. The paper does not exist. Both authors are real; they have never co-authored. Without verification, the citation would have gone into the paper.

## The verification protocol

For every entry in `annotated_bib.md`, do all of the following:

### Step 1 — Resolve a primary source

A primary source is one of:
1. Publisher page on the journal's website (DOI link).
2. Crossref DOI resolver: `https://api.crossref.org/works/<doi>`.
3. NBER working paper page: `https://www.nber.org/papers/wxxxxx`.
4. SSRN page: `https://papers.ssrn.com/sol3/papers.cfm?abstract_id=xxxxx`.
5. Author's personal website (for working papers).

Google Scholar entries are acceptable as a tertiary fallback; they aggregate from publishers and sometimes hallucinate too.

### Step 2 — Confirm structural fields

From the primary source, confirm:
- Authors match (order, spelling).
- Year matches.
- Journal name matches exactly (Crossref returns canonical title).
- Volume and pages match.
- DOI resolves to the same paper.

If any of these disagree, dig deeper. Most disagreements are simple errors (the assistant got the year wrong; fix). Some are unresolvable — the paper does not exist; delete and replace.

### Step 3 — Read the abstract

Open the paper. Read the abstract. Confirm the "finding" line in your bib entry matches what the paper actually claims.

Embed the abstract under each entry:

```markdown
### Smith, Jones, Wang 2024 — Title

**Citation**: ...
**Finding**: ...
**Relevance**: ...
**ID strategy**: ...
[verified 2026-05-16 via Crossref + AER publisher page]

<!-- Abstract:
We study X in Y data, finding that Z. The identification exploits A.
The headline coefficient is B (SE C). We rule out D through E.
-->
```

### Step 4 — Read the intro

Skim the introduction. Confirm the paper actually establishes what your bib entry says it does. Some papers are mis-summarized in search snippets (the abstract says X but the actual paper focuses on Y).

### Step 5 — Tag verified

Append `[verified YYYY-MM-DD via <sources>]` to the entry.

## Tools

- **Crossref API.** `https://api.crossref.org/works/<doi>`. Returns canonical metadata. Fast, free, reliable.
- **Semantic Scholar Graph API.** `https://api.semanticscholar.org/graph/v1/paper/<id>`. Useful for "cited by" graph traversal during lit review.
- **Google Scholar.** Web UI; no API. Acceptable as fallback verification source.
- **pdftotext (poppler-utils).** For reading actual PDFs locally. `brew install poppler` on macOS.
- **WebFetch tool.** Inside Claude, use WebFetch on publisher URLs to confirm landing pages.

## Red flags during verification

Pay extra attention when any of these appear:

- **"Forthcoming" with no volume / no SSRN URL.** Real forthcoming papers usually have an SSRN draft. If neither, suspect fabrication.
- **Co-author pairs that have never collaborated.** Two famous econometricians from different sub-fields rarely co-author. Cross-check on Google Scholar each author's page.
- **Implausible page ranges.** AER articles are typically 30+ pages; a 4-page AER article is unusual. JF articles are typically 35-50 pages.
- **Journal name slightly off.** "Journal of Accounting and Economics Research" is not a real journal; the real one is "Journal of Accounting and Economics" (no "Research"). Slight variants are a fabrication tell.
- **Year inconsistent with volume.** JAE volume 95 was published in 2023, not 2019. Crossref will catch this.
- **DOI that does not resolve.** Test every DOI via `https://doi.org/<doi>`. A 404 or "DOI not found" is a fabrication signal.

## Parallel verification pattern

For large bibs (>20 entries), spawn one subagent per 5 papers.

```
Subagent prompt:
You are verifying 5 bib entries. For each:
1. Resolve DOI via Crossref or publisher page.
2. Confirm authors, year, journal, volume, pages.
3. Read abstract; confirm "finding" line matches.
4. Read intro; confirm scope match.
5. Return verified block with abstract inline, OR mark as needs-replacement
   with the specific verification failure.

Here are the 5 entries: [paste]
```

Each subagent runs independently. Master synthesizes. Total wall time is the time of the slowest subagent (5-10 minutes per batch of 5), not the sum.

## Bulk caveats

- Verification finds errors. Do not panic when you find a fabrication; it is the protocol working. The shock is when you do not find any fabrications and you have only verified 5 papers — keep going.
- Do not "verify by Codex". Codex hallucinates too. Verification means primary source.
- Do not "verify by search snippet". A two-line snippet is not the paper.

## After Phase 8

Every entry has a `[verified]` tag. Every entry has an abstract block. Fabrications are documented in `dead_branches.md`. Replacements have themselves been verified.

If you skip this phase, you ship with fabrications. There is no other safety net.
