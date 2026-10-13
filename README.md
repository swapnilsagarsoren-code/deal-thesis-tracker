# M&A Value Realization Tracker (India)

**Can anyone outside tell whether the acquirer delivered what it publicly promised?** A cited, rule-based check of Axis Bank's purchase of Citi India's consumer business (closed 1 March 2023).

It is the check a CFO's office runs on its own deal after closing. It is a public-disclosure reconciliation, not proof of fraud, not an investment tool, and not a platform. For each promise it:

1. Records what Axis promised in its own documents, word for word, with the document and page.
2. Checks what Axis later reported (annual reports FY23–FY26, earnings calls) against it.
3. Labels every number by assurance level: audited note, annual-report management discussion, or call commentary.
4. Lets fixed rules in code set the status. AI never decides a status.
5. Shows gaps as gaps: "Unverifiable", "Different basis disclosed", "not disclosed". Never "failed" when data is missing.

## Status (14 Oct 2026)

- **Live tracker (full version, released 13 Oct 2026):** https://doting-zebra-426.convex.site. All 4 promises, the CFO close-out check, the RBI card-data check, the auditor check and the [method page](https://doting-zebra-426.convex.site/#methodology). The AI evidence finder is switched off on the live site.
- **App code (public):** https://github.com/swapnilsagarsoren-code/deal-thesis-tracker-app. This repo holds the planning docs, the handoff and the method; that one holds the code that runs the live site.
- **Build write-up:** https://hq.swapnilsoren.com/builds/deal-thesis-tracker
- **Built during** the GrowthX Build Sprint, 2–17 Oct 2026. Scope was cut from 4–5 deals to one deal in depth on 10 Oct (decision log rows 24–25).
- **The review is retrospective**, not outcome-blind: the deal was chosen and its results read in the same period ([methodology §6](docs/methodology.md)).

## Findings so far

| Promise (Axis's own words) | Status | Why |
|---|---|---|
| Complete the integration within 18 months | Delivered (management-reported) | Final migration reported on 14 Jul 2024, before the deadline. "Seamlessly" not tested. |
| Save 30–40% of Citi's operating costs within 2 years | Unverifiable | Success was later measured against an internal Board plan, not the public promise. Different basis disclosed. |
| Integration cost of ~₹2,000 crore over 18 months | Unverifiable | ₹1,323 crore disclosed for months 1–13; months 14–18 not disclosed. The estimate is last repeated in the FY23 annual report. |
| EPS and ROE accretive in CY24 | Unverifiable | No reviewed document says whether it happened. |

- **CFO close-out check:** 1 of 6 items a CFO's office needs to close out the deal is fully disclosed. None of the 4 items analysts asked about is fully disclosed.
- **RBI card data:** Citi's and Axis's card counts around the March 2023 transfer are shown, labelled "Not comparable on the available definitions".

## Who does what

| Step | Who | Why |
|---|---|---|
| Record the promise and target | A person, from Axis's own documents | No target is invented |
| Suggest passages in the filings | AI evidence finder (Claude Haiku). Built and tested on a test site; switched off on the live site | Reading 1,994 pages is where AI helps |
| Check the quote is word for word on its page | Code | Stops invented quotes |
| Confirm what a passage means | A person | "Verified" only means the words are on the page |
| Decide the status | Fixed rules in code | Pass/fail must be repeatable and checkable |

Statuses: Delivered / Partial / Missed / Too early / Unverifiable / No target committed.

## Scope (locked for the sprint)

- **In:** Axis × Citi only, in depth.
- **Out until after 17 Oct:** other deals (including RateGain × Sojern), the automatic filing-extraction pipeline, showing the AI disclosure timeline publicly, Forensic Scorecard, goodwill/purchase-price allocation, live search, AI "management action" suggestions, payments.

## Repo map

| File | What it holds |
|---|---|
| [`HANDOFF.md`](HANDOFF.md) | **Start here if you are an AI picking this up.** Current state, rules, next actions |
| [`docs/product-spec.md`](docs/product-spec.md) | Current product spec |
| [`docs/methodology.md`](docs/methodology.md) | How promises, targets and statuses are set, and the limits |
| [`docs/integration-cost-evidence.md`](docs/integration-cost-evidence.md) | The integration-cost evidence, page by page |
| [`docs/design.md`](docs/design.md) | Design tokens and UI rules |
| [`docs/decision-log.md`](docs/decision-log.md) | Every decision made, with the reason |
| [`docs/research-sources.md`](docs/research-sources.md) | M&A research used to ground the rubric |
| [`docs/stress-tests.md`](docs/stress-tests.md) | "Isn't this just Perplexity / ChatGPT?" — honest answers |
| [`docs/sprint-plan.md`](docs/sprint-plan.md) | Original 2 Oct plan (superseded; see HANDOFF §7) |
| [`docs/deals.md`](docs/deals.md) | Original deal shortlist (cut to one deal on 10 Oct) |
| [`docs/forensic-extraction.md`](docs/forensic-extraction.md) | Audited line items per claim type (written for non-banks; deferred) |
| [`mockup/index.html`](mockup/index.html) | Early clickable mockup (superseded by the live tracker) |
