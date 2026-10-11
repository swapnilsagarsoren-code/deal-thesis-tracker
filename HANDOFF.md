# HANDOFF — read this first

For any AI model or person continuing this project. Last updated: **11 Oct 2026, ~18:30 IST**.

## 1. Who you are working with, and who does what

- **Swapnil:** ex-Deloitte external auditor (5 years, manufacturing/automotive clients), now in a one-year MBA (IIM Lucknow IPMX). Builds this solo, as a placement story for finance transformation consulting (Accenture CFO&EV, Accenture Strategy, EY-Parthenon). Placement week starts **26 Oct 2026**.
- **Coding level:** beginner. All code is written by AI agents.
- **Roles:**
  - **Codex (terminal)** builds the app in `~/build-sprint-app`.
  - **Shaktimaan (the GrowthX Build Sprint AI agent)** reviews the product and plans the sprint.
  - **Claude** keeps this folder and the HQ site entry updated, sense-checks Codex and Shaktimaan, checks prod data (read-only Convex connection), and checks claims against the source PDFs.
- **Working style:** direct, critical, simple English, bullet points. More in `private/ai-working-notes.md` (local only).

## 2. What the product is (honest current state)

**M&A Value Realization Tracker (India).** It checks whether an Indian acquirer delivered what it publicly promised in a deal.
- It takes the company's own stated promises with exact quote, page, deadline and threshold.
- It checks later disclosures against them.
- Fixed rules in code decide the status: Delivered / Partial / Missed / Too early / Unverifiable / No target committed.
- **On prod today it has no AI.** Evidence is entered by hand.
- **On dev (since 11 Oct): an AI evidence finder** (Claude Haiku 5.5). Given a promise, it suggests passages from the 11 documents.
  - Code checks that every quote appears word for word on that page.
  - Nothing is shown publicly until a person accepts it; the rules decide the status.
  - AI never decides a status. When it finds nothing, it shows what it searched.
  - Live test results and limits: see the finder section at the end.
- **Positioning:** a CFO-office value-realization check, and a *public-disclosure reconciliation*. Not "forensic proof", not an investor tool, not a "platform". Detail: [`docs/product-spec.md`](docs/product-spec.md) (rewritten 11 Oct) and [`docs/design.md`](docs/design.md) (new 11 Oct).
- **Origin:** an Accenture consultant's on-campus session (1 Aug 2026). His points: merger savings across infra/licence/people, "cite the sources", human in the loop.

## 3. Where things stand (10 Oct 2026 evening; dev updates 11 Oct below)

**Live app:** https://doting-zebra-426.convex.site (Convex prod `doting-zebra-426`; dev `deafening-sparrow-421`, dev URL https://deafening-sparrow-421.convex.site). React + Vite + Convex. Code is in `~/build-sprint-app` (not on GitHub; not in this folder).
- One deal: Axis Bank buying Citi India consumer business (closed 1 Mar 2023). Two promises on prod:

| Promise | Source | Status | "What we found" (shown to visitors) |
|---|---|---|---|
| Integration: "complete the full integration seamlessly within 18 months post LD1" | 1 Mar 2023 call, p.5 (start date p.16) | **Delivered** (rev 3) | Management reported completing the final customer migration on 14 Jul 2024 (Q1FY25 call p.4), before the 1 Sep 2024 deadline. Seamlessness for customers not tested. |
| Cost savings: "30%-40% of Citi's operating expenses … over 2 years post closing" (basis CY2020 opex) | Deal presentation p.16 (closing date p.6) | **Unverifiable** (rev 1) | Management said synergies came "a bit ahead of the plan", measured against an internal six-quarter Board plan (Q4FY25 call p.13). The sources reviewed don't report the saving against the public 30–40% promise. |

- **Shipped on 10 Oct:**
  - Edit lock: saves need a server-side passcode in the `EDIT_PASSCODE` env var. Swapnil set it on prod and never shares it.
  - The edit screen shows only with `?edit=1`.
  - "Loading assessment" shows while data loads, and a retry message with no verdict if loading fails.
  - Each source link shows document title and date, has a 44px tap area, and uses `#page=N` (all 8 links checked: printed page = PDF page for these documents).
  - Separate citations for the promise and the start date.
  - Prod statuses and version numbers unchanged throughout. Snapshots before each ship are in `~/build-sprint-app/artifacts/` and `private/prod-snapshot-2026-10-10-pre-ship.md`.
- **Portfolio page live:** https://hq.swapnilsoren.com/builds/deal-thesis-tracker (repo `swapnilhq`). It shows both results, links to the live tracker, and has a "View the plan" button pointing to this repo.
- **Old mockup:** https://deal-thesis-tracker.vercel.app. Superseded; not linked anywhere new.

- **11 Oct, on dev only (not on prod):**
  - Two more promises, both **Unverifiable** via the unchanged status rules: integration budget ("Rs 2,000 crores pre-tax … over 18 months period", call p.8) and EPS/ROE accretion ("EPS & ROE accretive in CY24", deck p.5).
  - Reporting-basis panel for cost savings: overall **Different basis disclosed** (6 rows: metric, size, baseline, perimeter, period, measured-against), missing bridge, documents searched, blank reviewer fields.
  - "Considered — no target committed" list and "Not promises — closing-date pro forma figures" list.
  - Annual-report links FY23–FY26 verified (HTTP 200, page counts 376/466/504/520, cited text on cited page). Offsets: FY23 printed = PDF − 2; FY24/FY25 printed = PDF; FY26 printed = PDF + 1.
  - Stored in two NEW tables (`additionalPromise`, `reportingReview`); `deal` table and `shared/statusRules.ts` untouched. New save functions need the passcode and are locked to dev — that lock must be changed deliberately on prod release day.
  - Gotcha: a daily cron rewrites `assessment.asOf` on the `deal` rows at midnight IST. Snapshot comparisons must ignore that one field.
  - Integration-cost panel: ₹1,323 crore "Disclosed, months 1–13 of 18", no variance, exclusions collapsed.
  - **Phase 4.0/4.1 polish (deployed and seeded on dev, 11 Oct evening):**
    - Top summary table "What was promised vs what was reported" (4 rows, status badges from the rules), then the one-line finding: "1 delivered (management-reported), 3 cannot be verified from Axis's public sources…".
    - Integration threshold now "complete the full integration" (exact substring), with "'Seamlessly': not tested" shown; promise quote trimmed of "And number five,". Status still Delivered.
    - Integration-budget card: target "~₹2,000 crore pre-tax over 18 months (an estimate, not a cap)", deadline 31 Aug 2024, reported "₹1,323 crore for months 1–13; months 14–18 not disclosed", measured as of 31 Mar 2024, analyst refusal cited (Q1FY25 p.18). Status Unverifiable.
    - EPS/ROE card: "No reviewed document (annual reports FY24–FY26, Q4FY24–Q4FY25 calls) states whether the acquisition was EPS or ROE accretive…". Verified by text search: every "accretion" hit in those documents is about CET1 capital or deposits.
    - Auditor table shown once; empty finder timelines replaced by "Evidence finder: not run for this promise."; one date format.
  - **Decision (11 Oct): prod release waits until 16 Oct**, together with the final recheck of numbers and links. Release = snapshot prod → lift the dev-only lock on the new save/seed functions (passcode stays) → deploy → seed prod → snapshot → check live page at 390px/1440px.
  - **Decision (11 Oct): reviewer = Swapnil for now**; fill name/date on prod via `?edit=1` after release. Swap in a second reviewer if one becomes available.

**Honest weakness (Swapnil's own view, agreed):** so far it compares management's words with management's words. The fix is to test claims against evidence management doesn't control in the same way: annual-report figures, regulator data, and what management declined to disclose.

## 4. Findings so far (verified against the PDFs)

- **Cost savings: different basis disclosed.** The public promise was 30–40% of CY2020 Citi opex over 2 years. The later success claim is against an internal six-quarter Board plan. No bridge between the two is in the reviewed sources.
- **Integration cost** (full table: [`docs/integration-cost-evidence.md`](docs/integration-cost-evidence.md)):
  - Estimate: about ₹2,000 cr pre-tax over 18 months (1 Mar 2023 call p.8; repeated in FY23 AR PDF p.109, covering "transitional costs and integration expenses").
  - Disclosed: FY23 ₹217 cr (FY23 AR PDF p.109) and FY24 ₹1,106 cr (FY24 AR printed p.212 = PDF p.212, MD&A). That's ₹1,323 cr for 13 of 18 months.
  - Apr–Aug 2024 not disclosed. FY25 AR (PDF p.238) says only that integration costs fell.
  - Both figures are management narrative in the annual report, not confirmed as audited notes. It's unclear whether TSA costs are inside ₹1,106 cr.
- **Declined to disclose:**
  - Q1FY25 call, PDF p.18: an analyst asked for integration costs and retention ratios.
  - CFO's answer included "we don't share specific numbers", followed by a claim that every acquisition metric was "met or exceeded".
- **Analysts asked for numbers repeatedly** (call transcripts): attrition (closing call p.4, Adarsh Parasrampuria), CY24 accretion timing (p.11, Rahul Jain), retention and integration costs (Q1FY25 p.18, Anand Dama), synergy quantification (Q4FY25 p.13, Anand Swaminathan). No retention figure and no rupee/% realised-synergy figure appears in the FY24–FY26 annual reports or later calls (text search).
- **Synergy reporting stops being separable:** Q4FY25 call p.13 (Subrat Mohanty): "we no longer separate the Citi and the Axis book and monitor their progress". Management's own reason why accretion, retention and synergy can't be checked from outside after LD2.
- **No synergy number in any annual report FY23–FY26.** Closest: FY24 AR p.232, "70 synergy initiatives across cross-sell, deepening, sales productivity and cost rationalization" (deal call said "60+").
- **Accretion wording differs across deal-time documents:** deck p.5 "accretive in CY24" vs FY23 AR printed p.10 (PDF p.12) "accretive at the end of calendar year 2024". Different basis disclosed. No later report mentions accretion.
- **Axis never committed to publish integration numbers:** 1 Mar 2023 call p.7, "Right now we have not decided what we'll share and how much we'll share."
- **Market-share claim, corrected:** the deal deck's 11.4% → 16.2% is a share of credit-card *spends/advances* (p.14 footnote), not card counts. The 9.4M + 1.8M = 11.2M card figure is on p.10, with a consent cutoff of 20 Feb 2023.
- **External context (11 Oct research, for the story only; not evidence about Axis):** BCG (Nov 2025, 175 deals >$1bn, 2019–23) found acquirers that disclosed actual synergy realisation had ~6% higher relative TSR over the next two years. Never claim this effect applies to Axis.

**Interview line (defensible):** "Axis disclosed ₹1,323 crore of integration cost against a ~₹2,000 crore estimate, covering 13 of 18 months; when an analyst asked for the numbers, management said it doesn't share specific numbers, while saying every target was met. And its cost-saving success was measured against an internal plan, not the public 30–40% promise. I built a cited tracker that makes these evidence gaps visible without calling them failures."

## 5. Locked decisions (reasons in [`docs/decision-log.md`](docs/decision-log.md))

1. **Sprint scope (changed 10 Oct):** Axis–Citi in depth only. No new deals before 17 Oct.
2. Status is decided by deterministic code, never by an LLM. `shared/statusRules.ts` is not changed without an explicit decision.
3. Only targets the company itself stated publicly. Never invent a target; a gap is shown as a gap.
4. **Honest sequence:** promises are recorded exactly as stated and then checked. This was retrospective (deal picked and results read the same day), so don't claim "fixed before seeing results".
5. **Wording:** "public-disclosure reconciliation", not "forensic proof". "Different basis disclosed / Comparable / Insufficient detail", not "moved goalposts". "Unverifiable" or "cannot reconcile", never "failed", when data is missing.
6. Never call it a "platform" or say it "continuously tracks".
7. Silence is not failure. Not disclosing ≠ not delivering.
8. **Finder frozen (11 Oct, decision log row 36):** no further finder tuning before 17 Oct.

## 6. Guardrails for anyone changing the app

- **Dev and prod have separate databases.** A data change on dev doesn't reach prod. Use a one-off, idempotent internal migration: snapshot prod before, migrate, snapshot after. Or edit on prod through `?edit=1` with the passcode.
- **The passcode** is never pasted into chat, code, logs or commits. **The Anthropic API key** is entered only through hidden `read -s` prompts.
- **Loading or fetch failure must never show a verdict.**
- **`#page=N` uses the PDF page index.** Annual reports can differ from printed pages, so show "printed p. X (PDF p. Y)" when they differ. Keep the page label visible as text, because iOS Safari ignores `#page`.
- **No invented numbers.** Every number traces to a page. Label each number's assurance level: audited note, annual-report narrative, or call transcript.
- **Ask Codex for raw test output, not summaries.** Verify prod with the read-only Convex connection after every ship.

## 7. Next actions: 11–17 Oct (Shaktimaan's plan, adopted with Claude's edits)

1. **DONE 11 Oct:** research questions and source inventory frozen; retrospective sequence written (methodology §6).
2. **DONE 10–11 Oct (on dev) — check 3 (basis comparison):**
   - Put the public 30–40% promise next to the internal-plan statement.
   - For each, record metric, denominator, business perimeter, baseline, period and deadline.
   - Output: "Different basis disclosed", plus the CFO action: ask the synergy owner for the bridge from public baseline → internal plan → realised saving.
3. **DONE 11 Oct (on dev) — check 1 (integration-cost reconciliation).** Panel built and seeded; findings in docs/integration-cost-evidence.md. Still open: whether TSA costs are inside ₹1,106 cr (not confirmed by the annual reports).
4. **15 Oct, check 2 (bounded):** cards outstanding only, from RBI monthly tables (Axis, Citi, all banks around the closing date). Two-hour gate to confirm the definitions match. If they don't, publish "Not comparable on the available definitions" and stop. Never test the spend-share % with card counts.
5. **By 15 Oct: record the demo video** (agreed guardrail), so 16–17 Oct stay free for the release.
6. **By 14 Oct: show the dev site to 3 people** and log one comment from each (ship-check gap: zero users so far).
7. **16 Oct:** freeze scope, recheck every number and link, rehearse with someone who doesn't know the deal, prod release.
8. **17 Oct:** fix only demo blockers; backup recording.
- **Deferred:**
  - AI "silence timeline" (check 4)
  - goodwill / purchase-price allocation
  - bank-specific line-item map (`docs/forensic-extraction.md` is written for non-banks)
- **Cap the build at about 3 hours a day.** Case-interview prep had not started as of late Sep, and cases decide placement.

**3-minute demo order:**
1. Who needs this.
2. The public promise vs the internal-plan claim.
3. The cost table with assurance labels.
4. The RBI comparison, or why it was rejected.
5. Open a citation and show the fixed rule.
6. The CFO action and the limit of the conclusion.

## 8. Sprint goal (as submitted to GrowthX)

> Ship, by Oct 17, a working demo of an India M&A deal-thesis tracker covering 4–5 curated deals, where for at least 2 of them the AI extracts specific numbers from company filings … and a deterministic rule checks them against a threshold set before the outcome was known — producing a cited verdict.

**Honest gap against it:**
- 1 deal, not 4–5 (cut deliberately on 10 Oct; decision log rows 24–25).
- The review is retrospective: thresholds were not set before the outcome was known (methodology §6).
- The AI finds and quotes passages. It doesn't extract the number that sets a status: a person accepts each finding, and fixed rules set the status (rows 30–36).

Say so plainly if asked.

## 9. Where everything lives

| Thing | Location |
|---|---|
| This handoff and the planning docs | `~/Downloads/Work/deal-thesis-tracker` → GitHub `swapnilsagarsoren-code/deal-thesis-tracker` (public) |
| App code | `~/build-sprint-app` (local only) |
| Source PDFs (Axis reports, transcripts, decks) | `~/Downloads/growthxbuilddocs`; also uploaded to the claude.ai Project "M&A Tracker" (which is full) |
| HQ portfolio site | `~/Downloads/Work/swapnilhq` → GitHub `swapnilhq` (private) → Vercel → hq.swapnilsoren.com |
| Career notes | `~/Downloads/Work/Career-OS/master-repository/experience-bank/ai-products.md` (Product 4) |
| Private notes, prod snapshot | `private/` in this folder (gitignored) |
| Backup of pre-rebase local history | branch `backup-before-rebase-10oct` in this repo |
| Ship check (11 Oct) | claude.ai Project "M&A Tracker" (private) |

## 10. Private context

`private/` holds working style, goals, outreach contacts (send the demo to the Accenture contact only once it works) and calendar constraints. Not in the public repo.

## Release checklist additions (11 Oct)
- Phase 3.2 changed every passcode-protected function (including edit-save) from mutation/query to action: retest edit mode, reviewer save and finder run on dev before prod.
- Passcode lockout is global per India hour: 10 wrong guesses lock everyone (including you) for the rest of that hour.
- New save/seed/finder functions are locked to dev; lift deliberately on release day.
- Fill "Reviewed by" (Swapnil, date) on the reporting-basis panel after release.
- Known small fix: FY25 section map keeps PDF 275–276 as MD&A (per contents page) though they hold the governance certificate; set MD&A end to 274 before release.
- Methodology page section 4 ("Today there is no AI") must be updated when the finder ships; proposed wording is in the 11 Oct chat (how the AI is used, verified = words on the page, held-out 4/6, nothing public until accepted).

## AI evidence finder — status 11 Oct, 18:30 IST (deployed on dev; FINDER_ENABLED still off; not on prod)
- Built by Codex in phases 1 → 3.7; checked by Claude after each phase. Your Mac: 160/160 tests at Phase 3.6; statusRules and ledger values unchanged.
- What it does: per promise, searches 11 Axis documents (1,994 pages; uploaded to dev), Claude Haiku 5.5 returns verbatim quotes only; code verifies every quote word for word, labels each document, checks figures against a hand-verified ledger, hides anything unverified from the public until Swapnil accepts it, flags prompt-injection text, labels assurance by annual-report section.
- Live-test fixes on 11 Oct: `temperature` removed (rejected for this model); thinking disabled and output cap 1,000 (replies were being cut off); rarity-weighted search; page priority changed to "top 3 by relevance + 1 slot for MD&A/notes"; one exact-copy retry on a word-for-word reject.
- **Live scores (Haiku 5.5):** practice set (tuned on) 7/10, 8/10, 7/10; held-out set (frozen before running) 4/6 found and 3/3 correctly found no figure. Total live-test cost ≈ ₹10.
- **Key limit:** "verified" means only "these words are on that page". No invented quote or number passed in any run, but 4 verified passages were real yet off-question (e.g. ₹1,106 cr returned for a synergy question; an analyst's question quoted as a report). Hence: a person must accept every finding.
- Cost controls: kill switch (FINDER_ENABLED off by default), passcode + dev-only, 5 runs / 60 calls / 600k tokens per day, 12 calls per run, 2-min cooldown, $5 prepaid credit with auto-reload off.
- Run files: `~/build-sprint-app/artifacts/finder/live-eval-run*.json`.
