# Product spec

## Problem

When a company buys another, management announces a "thesis" with promises: cost savings, cross-selling, market share. Media covers the announcement. Almost nobody goes back years later to check if the promises came true — and when they do, they judge with hindsight.

## Core idea

For each deal, write a **test plan** before looking at the outcome, then check it against audited numbers.

### Test-plan template (fixed, same for every deal)

| Field | Example |
|---|---|
| Stated thesis | "Combining quick-commerce with food delivery lowers delivery cost per order" |
| Specific claims | "₹X cr cost synergies by FY25" |
| Evidence sources | Annual report FY25, Q4 investor deck |
| Metric | Reported synergy / SG&A as % of revenue |
| Threshold (pre-registered) | Delivered ≥ 80% of target; Partial 40–80%; Missed < 40% |
| Verdict | Delivered / Partial / Missed / Too early / Unverifiable |

## Architecture: extract-then-compute

```
Filing PDF ──► LLM extracts number + exact citation ──► stored value
                                                         │
Pre-registered threshold (human, written earlier) ──────►│
                                                         ▼
                                           Plain code compares → verdict
```

- **Quantifiable claim:** LLM only extracts. Code decides. Verdict is reproducible.
- **Non-quantifiable claim** (e.g. "editorial independence"): LLM gives a judgment, shown with a "lower confidence" label.
- **Source-pinning:** every extracted number stores document, page, and quote so a human can check it in seconds. This is the main guard against extraction hallucination.

This pattern mirrors two reference designs discussed during planning: an AI tax-prep product (deterministic calculation engine + LLM for interview/explanation) and a spreadsheet model auditor (rule engine finds issues + LLM explains and ranks them).

## Two halves of the product

| | Reviewed core | Live search (post-sprint) |
|---|---|---|
| Deals | Curated set | Any deal a user types |
| Human check | Yes | No |
| Tag in UI | Green "REVIEWED · HUMAN-VERIFIED" | Amber "AUTO-GENERATED · UNREVIEWED" |
| Engine | Filings + extraction pipeline | Linkup search API + same pipeline |
| Honest failure | — | "Unverifiable — no strong public signal found" |

## Forensic Extraction layer (sprint)

Uses the builder's audit background: for each claim type, a fixed list of financial-statement line items the AI must check. Goodwill impairment is checked for every deal as a universal red flag. See [`forensic-extraction.md`](forensic-extraction.md).

## Forensic Scorecard (roadmap, not sprint)

Five scored dimensions shown next to each verdict:

1. **Synergy type** — cost vs revenue (research consistently finds cost synergies are delivered far more often than revenue synergies)
2. **Threshold specificity** — did management give a number and date, or vague language?
3. **Acquirer experience** — serial acquirers tend to integrate better
4. **Integration risk flags** — culture, systems, regulatory
5. **Timeline realism** — promised timing vs typical integration time

Grounded in published Bain / BCG / Accenture M&A research ([`research-sources.md`](research-sources.md)).

## Features that survive the "why not just ChatGPT" test

See [`stress-tests.md`](stress-tests.md). Short version: pre-registered thresholds, a persistent ledger, verdict history over time, source-pinned numbers, and named human review. Generic AI chat has none of these.

## Verdict history (roadmap)

Show how a verdict changed as new evidence came in (e.g. "Too early" in 2024 → "Partial" in 2026). Strongest single answer to "why not Perplexity" because chat tools keep no state between sessions. Not in sprint scope.

## Tech (suggested)

- Static site on Vercel (existing research-lab site)
- Serverless function for LLM calls (keeps API key off the client)
- JSON files as the data store for the sprint — no database needed for 4–5 deals
- JavaScript over Python, given builder's skill level

## Methodology page (if time)

Public page explaining rubric, thresholds and verdict rules. Cheap to build; adds trust.
