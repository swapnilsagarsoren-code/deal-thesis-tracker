# Product spec — M&A Value Realization Tracker (India)

## Problem

After an acquisition, management promises value: cost synergies, margin targets, break-even dates. The CFO's office is usually responsible for tracking whether that value arrives and reporting it to the board. In practice the link between the original promise, the operating numbers and the reported financials gets lost across decks, filings and earnings-call commentary. From outside, almost nobody checks — and when they do, they judge with hindsight.

## Who it is for (framing)

A prototype of the synergy-tracking and value-reporting tool a CFO's office or integration team would use, demonstrated on public Indian deals. It shows three finance-transformation skills: tracing a strategic promise to the line in the accounts where it should appear, building controls so every status can be checked, and reporting value realization clearly.

## Core idea

For each deal, build a test plan **before** looking at the outcome, then check it against audited numbers.

### Test-plan template (same for every deal)

| Field | Example (Axis–Citi) |
|---|---|
| Stated thesis | Grow the cards and affluent franchise; accretive to EPS and ROE |
| Company-stated target | Cost savings of 30–40% of Citi opex over 2 years |
| Source | Axis Bank investor presentation, 1 Mar 2023 |
| Financial-statement line | Operating expenses (employee + other) of the acquired business |
| Threshold (pre-registered) | e.g. Delivered ≥ 30%; Partial 15–30%; Missed < 15% |
| Status | Delivered / Partial / Missed / Too early / Unverifiable / No target committed |

## Value-driver tree (built on financial-statement lines)

Each deal's stated targets are grouped under value drivers, and each driver is tied to where it shows up in the accounts:

```
Deal thesis
├── Revenue        → segment revenue, fee income, customer counts (where disclosed)
├── Cost           → COGS, SG&A, employee cost, integration / one-time costs
└── Capital & risk → goodwill and impairment, leverage, ROE / ROCE, EPS
```

Rules:
- Only targets the company itself stated are scored.
- A driver with no company target is shown as **"No target committed — not scored"**. That gap is itself a finding: it shows what management avoided promising.
- The financial-statement mapping comes from [`forensic-extraction.md`](forensic-extraction.md).

## Architecture: extract-then-compute

```
Filing PDF ──► LLM extracts number + exact citation ──► stored value
                                                         │
Pre-registered threshold (human, written earlier) ──────►│
                                                         ▼
                                           Plain code compares → status
```

- **Quantifiable target:** LLM only extracts. Code decides. Status is reproducible.
- **Non-quantifiable claim:** LLM gives a judgment, labelled "lower confidence".
- **Source-pinning:** every extracted number stores document, page and quote. This is the main guard against extraction errors.
- **Control design (audit-built):** extraction is kept separate from judgment; ambiguous cases are flagged for human review. Lead with this in interviews.

## Screens (sprint)

1. **Deal overview** — thesis, deal value, timeline, overall status
2. **Value-driver tree** — drivers mapped to financial-statement lines, each with its status
3. **Evidence page** — per target: promise, source, baseline, target, deadline, actual, quote, variance, confidence
4. **CFO value-realization summary** — driver | stated target | actual | variance | status | source. Status from code only.

Optional, last: a 3-bullet AI summary of the code's results. It may never change a status.

## Not in the sprint (roadmap)

- **Live search** for any deal (Linkup API), tagged "auto-generated, unreviewed"
- **Forensic Scorecard** — synergy type, target specificity, acquirer experience, integration risk, timeline realism (grounded in Bain / BCG / Accenture research, see [`research-sources.md`](research-sources.md))
- **Status history** — how a status changes as new filings arrive; the strongest answer to "why not Perplexity"
- **Management-action hypotheses** — dropped for now: an outsider's AI guessing why a public company missed a target is speculation

## Why not just ChatGPT / Perplexity

See [`stress-tests.md`](stress-tests.md). Short version: pre-registered thresholds, statuses from code, every number linked to its source, an audit-built map of where value shows up in the accounts, and a record kept over time.

## Tech (suggested)

- Static site on Vercel (already live for the mockup)
- Serverless function for LLM calls (keeps the API key off the client)
- JSON files as the data store — no database needed for 4 deals
- JavaScript over Python, given the builder's skill level
