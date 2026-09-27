# India Deal-Thesis Tracker

**Did the merger deliver what management promised?**

An India-focused M&A tracker. For each deal it records what the acquirer publicly promised (synergy targets, cost savings, revenue claims), sets a pass/fail threshold *before* the outcome is known, then checks the promise against real numbers from audited filings.

> Blume's series decodes the announcement. This tracker decodes what happened next.

## Status

- **Stage:** planning done, build not started (as of 28 Sep 2026)
- **Build window:** GrowthX Build Sprint, 2–17 Oct 2026
- **Mockup:** [`mockup/index.html`](mockup/index.html) — open in a browser, fully clickable, uses sample data

## What the AI does (and what it does not)

| Step | Who does it | Why |
|---|---|---|
| Find the promised number in a filing | AI (LLM) | Reading long, messy PDFs is where an LLM earns its place |
| Cite the exact source (doc, page, line) | AI + stored reference | Makes every number checkable |
| Compare number to threshold | Plain code | A pass/fail call should never be an AI guess |
| Set the threshold | Human, in advance | Stops hindsight bias — audit discipline |
| Judge claims with no clean number | AI, labeled "lower confidence" | Honest about where judgment is fuzzy |

## Sprint scope (locked)

- 4–5 curated deals
- At least 2 with the full AI extract → code check → cited verdict pipeline
- Verdicts: Delivered / Partial / Missed / Too early / Unverifiable

**Out of scope for the sprint:** live search for any deal (Linkup), Forensic Scorecard, payments.

## Repo map

| File | What it holds |
|---|---|
| [`HANDOFF.md`](HANDOFF.md) | **Start here if you are an AI picking this up.** Current state, rules, next actions |
| [`docs/product-spec.md`](docs/product-spec.md) | Full product design and architecture |
| [`docs/sprint-plan.md`](docs/sprint-plan.md) | Scope, time estimate, day-by-day plan, sprint goal statement |
| [`docs/decision-log.md`](docs/decision-log.md) | Every decision made, with the reason |
| [`docs/stress-tests.md`](docs/stress-tests.md) | "Isn't this just Perplexity / ChatGPT?" — honest answers |
| [`docs/forensic-extraction.md`](docs/forensic-extraction.md) | Which audited line items to check per claim type |
| [`docs/research-sources.md`](docs/research-sources.md) | M&A research used to ground the rubric |
| [`docs/deals.md`](docs/deals.md) | Candidate deal list and selection rule |
| [`mockup/index.html`](mockup/index.html) | Clickable mobile mockup |
