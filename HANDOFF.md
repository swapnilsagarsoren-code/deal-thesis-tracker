# HANDOFF — read this first

For any AI model or person continuing this project. Last updated: 28 Sep 2026.

## 1. Who you are working with

- **Swapnil** — ex-Deloitte external auditor (5 years, manufacturing/automotive clients), now in a one-year MBA. Builds this solo.
- **Coding level:** beginner Python. Builds with AI coding help (Codex / Claude). Prefer simple JS + static hosting over complex stacks.
- **Working style:** see `private/ai-working-notes.md` (local only) for how he wants feedback delivered.

## 2. What the product is (one paragraph)

An India M&A tracker. For each deal: record what the acquirer publicly promised, pre-set a pass/fail threshold, have an LLM **extract** the actual number from audited filings with a source citation, and let **plain code** compare number vs threshold to give a verdict. Full detail: [`docs/product-spec.md`](docs/product-spec.md).

## 3. Where things stand

- Planning and stress-testing: **done**
- Clickable mockup: **done** ([`mockup/index.html`](mockup/index.html), sample data only)
- Real build: **not started**
- Sprint: GrowthX Build Sprint, **2–17 Oct 2026** (Oct 10 unavailable)

## 4. Locked decisions — do not reopen without new information

1. Build curated deals first. Live "search any deal" (Linkup API) is **post-sprint**.
2. Scope for Oct 17: **4–5 deals, ≥2 with full extract → code-check → cited verdict.**
3. Verdict = deterministic code, never the LLM's opinion, whenever a real number exists.
4. Thresholds are written down **before** running extraction (pre-registration).
5. Only deals whose promises appear in the acquirer's own public sources (filings, investor decks, annual reports, earnings calls).
6. No payment / monetization build during the sprint.
7. Forensic Scorecard (5 dimensions) → roadmap slide only, not built in sprint.

Reasons for each: [`docs/decision-log.md`](docs/decision-log.md).

## 5. Guardrails

- **Scope creep.** Any new feature idea gets checked against the locked scope (section 4) and the time estimate before work starts.
- **Wrong terms.** The curated deals are *few-shot examples + a grounded rubric*, **not "training data"**. No model is trained.

## 6. Open items / next actions (in order)

1. Confirm with GrowthX that extending an existing project is allowed in the sprint. *(unconfirmed)*
2. Swapnil writes the exact line items per claim type in [`docs/forensic-extraction.md`](docs/forensic-extraction.md) — only he can do this well (audit background).
3. Pick final 4–5 deals from [`docs/deals.md`](docs/deals.md); collect source filings for each.
4. Write pre-registered thresholds for each claim *before* any extraction run.
5. Build the extraction prompt + source-pinning; test on 2 deals.
6. Write the threshold-check code.
7. Swap mockup's hardcoded arrays for real results.
8. Test, fix, buffer.
9. (If time) Public methodology page.

Time estimate and day plan: [`docs/sprint-plan.md`](docs/sprint-plan.md).

## 7. Sprint goal (as submitted to GrowthX)

> Ship, by Oct 17, a working demo of an India M&A deal-thesis tracker covering 4–5 curated deals, where for at least 2 of them the AI extracts specific numbers from company filings (e.g. cost-synergy targets, headcount claims) and a deterministic rule checks them against a threshold set before the outcome was known — producing a cited verdict (on-track / missed / too early).
>
> Longer-term, the product should let anyone check any live deal this way, not just pre-loaded ones — but that needs a search integration not yet built, so it is explicitly out of scope for this sprint.
>
> What this build proves: the AI does real analytical work — extraction plus rule-based checking — not just summarizing filings or chatting about a deal, which is what separates it from asking ChatGPT or Perplexity the same question.

## 8. Private context

A `private/` folder exists locally (not in the public repo — see `.gitignore`) with personal context: working style, outreach contacts, calendar constraints, career goals. Ask Swapnil for it if you need it.
