# HANDOFF — read this first

For any AI model or person continuing this project. Last updated: 2 Oct 2026.

## 1. Who you are working with

- **Swapnil** — ex-Deloitte external auditor (5 years, manufacturing/automotive clients), now in a one-year MBA. Builds this solo.
- **Coding level:** beginner Python. Builds with AI coding help (Codex / Claude). Prefer simple JS + static hosting over complex stacks.
- **Working style and goals:** see `private/ai-working-notes.md` (local only).

## 2. What the product is

**M&A Value Realization Tracker (India)** — a prototype of the tool a CFO's office uses after an acquisition to track whether promised synergies are arriving.

For each deal: take the company's own stated targets, map each to the financial-statement line where it should appear, pre-set a pass/fail threshold, have an LLM **extract** the actual number from filings with a source citation, and let **plain code** decide the status.

**Positioning:** CFO-office value realization and finance controls — not an investor or deals tool. Lead with the business problem and the control design, not the AI. Full detail: [`docs/product-spec.md`](docs/product-spec.md).

## 3. Where things stand

- Planning, stress-testing, clickable mockup: **done**
- Mockup live at https://deal-thesis-tracker.vercel.app (Vercel auto-deploys the `mockup/` folder from `main`)
- Real build: **started 2 Oct 2026**
- Sprint: GrowthX Build Sprint, **2–17 Oct 2026** (Oct 10 unavailable)

## 4. Locked decisions — do not reopen without new information

1. Curated deals only. Live "search any deal" (Linkup API) is **post-sprint**.
2. Scope for Oct 17: **4 deals; Axis–Citi and RateGain–Sojern get the full extract → code-check → cited status pipeline.**
3. Status = deterministic code, never the LLM's opinion, whenever a real number exists.
4. Thresholds are written down **before** running extraction (pre-registration).
5. Only targets the company itself stated in public sources. Never invent a target — a value driver with no company target is shown as "No target committed".
6. Value-driver tree is built on **financial-statement lines** (revenue, COGS, SG&A, employee cost, goodwill/impairment, return on capital).
7. No payments, no Forensic Scorecard, no AI "management action" suggestions in the sprint.
8. Do not describe it as a "platform" or as "continuously tracking" — it is a working prototype.

Reasons for each: [`docs/decision-log.md`](docs/decision-log.md).

## 5. Guardrails

- **Scope creep.** Any new feature idea gets checked against section 4 and the time estimate before work starts.
- **Wrong terms.** The curated deals are *few-shot examples + a grounded rubric*, **not "training data"**. No model is trained.
- **No invented numbers.** Every number on screen must trace to a company source or be clearly marked sample.

## 6. Next actions (in order)

1. Confirm with GrowthX that extending an existing project is allowed in the sprint. *(unconfirmed)*
2. Swapnil finalises the line items per claim type in [`docs/forensic-extraction.md`](docs/forensic-extraction.md). This is also the backbone of the value-driver tree.
3. Pick the other 2 deals from [`docs/deals.md`](docs/deals.md); collect source filings for all 4.
4. Build each deal's value-driver tree and write pre-registered thresholds, dated, *before* any extraction run.
5. Build the extraction prompt + source-pinning; test on Axis–Citi and RateGain–Sojern.
6. Write the threshold-check code.
7. Replace the mockup's hardcoded data with real results; add the tree and summary-table screens.
8. Test, fix, buffer. (If time) methodology page; optional 3-bullet AI summary that never changes a status.

Time estimate and day plan: [`docs/sprint-plan.md`](docs/sprint-plan.md).

## 7. Sprint goal (as submitted to GrowthX)

> Ship, by Oct 17, a working demo of an India M&A deal-thesis tracker covering 4–5 curated deals, where for at least 2 of them the AI extracts specific numbers from company filings (e.g. cost-synergy targets, headcount claims) and a deterministic rule checks them against a threshold set before the outcome was known — producing a cited verdict (on-track / missed / too early).
>
> Longer-term, the product should let anyone check any live deal this way, not just pre-loaded ones — but that needs a search integration not yet built, so it is explicitly out of scope for this sprint.
>
> What this build proves: the AI does real analytical work — extraction plus rule-based checking — not just summarizing filings or chatting about a deal, which is what separates it from asking ChatGPT or Perplexity the same question.

The product was renamed and reframed on 2 Oct 2026 (see decision log); the submitted goal still holds.

## 8. Private context

A `private/` folder exists locally (not in the public repo — see `.gitignore`) with working style, goals, outreach contacts and calendar constraints. Ask Swapnil for it if you need it.
