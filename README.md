# M&A Value Realization Tracker (India)

**Did the merger deliver the value management promised — and where should it show up in the accounts?**

A prototype of the synergy-tracking and value-reporting tool a CFO's office needs after an acquisition. For each Indian deal it:

1. Takes the targets the company itself stated (filings, investor decks, earnings calls)
2. Maps each target to the financial-statement line where it should appear (revenue, COGS, SG&A, employee cost, goodwill, return on capital)
3. Sets a pass/fail threshold *before* the outcome is known
4. Has AI extract the actual number from later filings, with the exact source quote
5. Lets plain code compare actual vs threshold and report the status

## Status

- **Stage:** build started 2 Oct 2026 (GrowthX Build Sprint, 2–17 Oct 2026)
- **Live mockup:** https://deal-thesis-tracker.vercel.app — sample screens, companies' own stated targets, no verdicts yet

## Who does what

| Step | Who does it | Why |
|---|---|---|
| Find the promised and actual numbers in filings | AI (LLM) | Reading long, messy PDFs is where an LLM earns its place |
| Cite the exact source (doc, page, quote) | AI + stored reference | Every number is checkable in seconds |
| Map the target to a financial-statement line | Human (audit-built rules) | Knowing where a synergy shows up in the accounts is the expertise |
| Set the threshold | Human, in advance | Stops hindsight bias |
| Compare actual to threshold | Plain code | A pass/fail call should never be an AI guess |

Statuses: Delivered / Partial / Missed / Too early / Unverifiable / No target committed.

## Sprint scope (locked)

- 4 curated deals; **Axis–Citi** and **RateGain–Sojern** get the full extract → code check → cited status pipeline
- Value-driver tree per deal, built on financial-statement lines
- CFO value-realization summary: driver | stated target | actual | variance | status | source

**Out of scope for the sprint:** live search for any deal, Forensic Scorecard, AI-suggested "management actions", payments.

## Repo map

| File | What it holds |
|---|---|
| [`HANDOFF.md`](HANDOFF.md) | **Start here if you are an AI picking this up.** Current state, rules, next actions |
| [`docs/product-spec.md`](docs/product-spec.md) | Product design, value-driver tree, architecture |
| [`docs/sprint-plan.md`](docs/sprint-plan.md) | Scope, time estimate, day-by-day plan |
| [`docs/decision-log.md`](docs/decision-log.md) | Every decision made, with the reason |
| [`docs/stress-tests.md`](docs/stress-tests.md) | "Isn't this just Perplexity / ChatGPT?" — honest answers |
| [`docs/forensic-extraction.md`](docs/forensic-extraction.md) | Which audited line items to check per claim type |
| [`docs/research-sources.md`](docs/research-sources.md) | M&A research used to ground the rubric |
| [`docs/deals.md`](docs/deals.md) | Deal shortlist and selection rule |
| [`mockup/index.html`](mockup/index.html) | Clickable mobile mockup (deployed on Vercel) |
