# Phase 8 — Citation Verification

## Objective

Read every paper you cite, at minimum abstract + intro. Catch fabricated entries. Tag every verified entry in `annotated_bib.md`.

## Inputs

- Updated `annotated_bib.md` after Phase 7.

## Outputs

- Same file with `[verified YYYY-MM-DD via <source>]` tag on every entry.
- Verified abstract embedded as a hidden block under each entry.
- Replacement entries for any fabrications detected (which then start the verification loop again for the replacements).

## Why this is mandatory

LLM agents fabricate citations. The original project caught 6+ fabrications across multiple rounds: plausible-sounding author + year + journal combinations that did not exist. Some had real authors and real journals but wrong year, wrong volume, or wrong claim attribution. The patterns are subtle enough to slip past a casual read.

A fabricated citation in a published paper is a career-affecting event. The protocol exists because the failure mode is real and serious.

## The protocol

### Step 1 — For each entry, find a primary source

A primary source is one of:
1. Publisher page on the journal's website (DOI link).
2. Crossref DOI resolver: `https://api.crossref.org/works/<doi>`.
3. NBER working paper page: `https://www.nber.org/papers/wxxxxx`.
4. SSRN page: `https://papers.ssrn.com/sol3/papers.cfm?abstract_id=xxxxx`.
5. Author's personal website (if recent, often has working-paper PDF).
6. Google Scholar entry (acceptable as backup; not primary).

```python
import requests
def verify_doi(doi):
    r = requests.get(f"https://api.crossref.org/works/{doi}")
    if r.status_code == 200:
        data = r.json()["message"]
        return {
            "authors": data["author"],
            "year": data["issued"]["date-parts"][0][0],
            "journal": data.get("container-title", ["unknown"])[0],
            "title": data["title"][0],
            "volume": data.get("volume"),
            "pages": data.get("page"),
        }
    return None
```

### Step 2 — Confirm against bib entry

For every entry, confirm:
- Authors match (order, spelling).
- Year matches.
- Journal name matches exactly (Crossref returns canonical title).
- Volume / pages match (catches "wrong year, same journal" errors).
- DOI resolves.

If any of these disagree, dig deeper. Most disagreements are easily resolved (the assistant got the year wrong; fix). Some are unresolvable -- the paper does not exist.

### Step 3 — Read the abstract

Open the paper. Read the abstract. Confirm the "finding" line in the bib entry matches what the paper actually claims.

Embed the abstract as a hidden block in the bib entry:

```markdown
### Smith, Jones, Wang 2024 — Title

**Citation**: ...
**Finding**: ...
**Relevance**: ...
**ID strategy**: ...
[verified 2026-05-16 via Crossref + JAR publisher page]

<!-- Abstract:
We study X in Y data, finding that Z. The identification exploits A.
The headline coefficient is B (SE C). We rule out D through E.
-->
```

The hidden block lets you sanity-check your prose citations later: did you cite this paper for the right claim?

### Step 4 — Read the intro

Skim the introduction. Confirm the paper actually establishes what your bib entry says it does. Some papers are mis-summarized in search snippets (e.g., the abstract says X but the actual paper focuses on Y, with X as a side result).

If the intro contradicts the bib's "finding" line, fix the bib entry to match the intro. If the paper does not establish anything close to what you cited it for, replace the entry.

### Step 5 — Tag and move on

When all checks pass, append `[verified YYYY-MM-DD via <sources>]` to the entry.

### Step 6 — Handle non-existent papers

If you cannot find the paper through any primary source:
1. Google the exact title in quotes.
2. Search Semantic Scholar API for the title.
3. Search the listed authors' personal websites for working papers.
4. Search NBER + SSRN for an early version.

If none of these turn up the paper, it is fabricated. Delete the entry. Find a replacement that covers the same claim. Start the verification protocol for the replacement.

Log the fabrication in `dead_branches.md`:

```markdown
## Fabrication: <fake citation>
Caught Phase 8. No Crossref hit, no Google Scholar hit, no publisher
page. Replaced with <real citation> for the [audit-monitoring channel].
```

## Parallel pattern for speed

Spawn one subagent per 5 papers. Each subagent verifies 5, returns a verified block. Master synthesizes.

Subagent prompt template:

```
You are verifying 5 bib entries. For each:
1. Resolve DOI via Crossref or publisher page.
2. Confirm authors, year, journal, volume, pages.
3. Read abstract; confirm "finding" line matches.
4. Read intro; confirm scope match.
5. Tag verified or flag as needs-replacement.

Here are the 5 entries:
[paste]

Return a verified block with abstracts inline.
```

## Red flags during verification

- "Forthcoming" with no volume / no SSRN URL — usually fabricated. Always confirm against SSRN.
- Co-author pairs that are unusual (two famous econometricians from different fields who have never collaborated before).
- Page ranges with implausible spans (a 4-page AER article — AER articles are typically 30+ pages).
- Journal name slightly off ("Journal of Accounting and Economics Research" — not a real journal; the real one is "Journal of Accounting and Economics").
- Specific DOI that does not resolve.
- Year inconsistent with volume (Volume 95 of JAE was not published in 2019).

When any red flag fires, escalate: open the publisher page directly. If still no hit after a thorough search, treat as fabricated.

## What success looks like

- Every entry in `annotated_bib.md` has a `[verified]` tag.
- Every entry has an abstract embedded as a hidden block.
- Any fabrications found are documented in `dead_branches.md`.
- Replacements have themselves been verified.

## What failure looks like

- "I marked verified because Codex said the paper exists." Codex hallucinates too. Verification means primary source.
- "I marked verified after reading a search snippet." A snippet is not an abstract.
- "I cannot find the paper but the title sounds real." Real titles also resolve to primary sources. If you cannot find it, it does not exist.

## Pitfalls

- Verifying author + year but not reading the abstract. Mis-attributed claims are common — the citation is real but the bib entry's "finding" line is wrong.
- Verifying in bulk via a subagent that just confirms hits without dropping the abstract. The abstract is the artifact.
- Skipping verification on "well-known papers". Even well-known papers can have wrong-year errors in your bib. The protocol is uniform.

## Time budget

1-2 days for a 25-paper bib. Parallel subagents cut this to 4-6 hours of wall clock.
