# HANDOFF — read this first

For any AI model or person continuing this project. Last updated: **10 Oct 2026, ~19:00 IST**.

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
- **Today it has no AI.** Evidence is entered by hand.
- **Planned AI role (not built):** you give a filing; AI finds the sentence about a chosen promise and returns the quote, page, any number, and what that number is measured against (its base and period).
  - Code checks that the quote appears word for word on that page.
  - A human confirms, then the rules decide.
  - AI never decides a status.
  - When it finds nothing, it must show what it searched.
- **Positioning:** a CFO-office value-realization check, and a *public-disclosure reconciliation*. Not "forensic proof", not an investor tool, not a "platform". Detail: [`docs/product-spec.md`](docs/product-spec.md).
- **Origin:** an Accenture consultant's on-campus session (1 Aug 2026). His points: merger savings across infra/licence/people, "cite the sources", human in the loop.

## 3. Where things stand (10 Oct 2026 evening)

**Live app:** https://doting-zebra-426.convex.site (Convex prod `doting-zebra-426`; dev `deafening-sparrow-421`, dev URL https://deafening-sparrow-421.convex.site). React + Vite + Convex. Code is in `~/build-sprint-app` (not on GitHub; not in this folder).
- One deal: Axis Bank buying Citi India consumer business (closed 1 Mar 2023). Two promises:

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
- **Market-share claim, corrected:** the deal deck's 11.4% → 16.2% is a share of credit-card *spends/advances* (p.14 footnote), not card counts. The 9.4M + 1.8M = 11.2M card figure is on p.10, with a consent cutoff of 20 Feb 2023.

**Interview line (defensible):** "Axis disclosed ₹1,323 crore of its ₹2,000 crore integration budget for 13 of 18 months, then stopped; when an analyst asked for the numbers, it declined while saying every target was met. And its cost-saving success was measured against an internal plan, not the public 30–40% promise. I built a cited tracker that makes these evidence gaps visible without calling them failures."

## 5. Locked decisions (reasons in [`docs/decision-log.md`](docs/decision-log.md))

1. **Sprint scope (changed 10 Oct):** Axis–Citi in depth only. No new deals before 17 Oct.
2. Status is decided by deterministic code, never by an LLM. `shared/statusRules.ts` is not changed without an explicit decision.
3. Only targets the company itself stated publicly. Never invent a target; a gap is shown as a gap.
4. **Honest sequence:** promises are recorded exactly as stated and then checked. This was retrospective (deal picked and results read the same day), so don't claim "fixed before seeing results".
5. **Wording:** "public-disclosure reconciliation", not "forensic proof". "Different basis disclosed / Comparable / Insufficient detail", not "moved goalposts". "Unverifiable" or "cannot reconcile", never "failed", when data is missing.
6. Never call it a "platform" or say it "continuously tracks".
7. Silence is not failure. Not disclosing ≠ not delivering.

## 6. Guardrails for anyone changing the app

- **Dev and prod have separate databases.** A data change on dev doesn't reach prod. Use a one-off, idempotent internal migration: snapshot prod before, migrate, snapshot after. Or edit on prod through `?edit=1` with the passcode.
- **The passcode** is never pasted into chat, code, logs or commits.
- **Loading or fetch failure must never show a verdict.**
- **`#page=N` uses the PDF page index.** Annual reports can differ from printed pages, so show "printed p. X (PDF p. Y)" when they differ. Keep the page label visible as text, because iOS Safari ignores `#page`.
- **No invented numbers.** Every number traces to a page. Label each number's assurance level: audited note, annual-report narrative, or call transcript.
- **Ask Codex for raw test output, not summaries.** Verify prod with the read-only Convex connection after every ship.

## 7. Next actions: 11–17 Oct (Shaktimaan's plan, adopted with Claude's edits)

1. **11 Oct:** freeze the research questions and source inventory; write down the retrospective sequence honestly. *(Loading fix and integration wording already shipped 10 Oct.)*
2. **12 Oct, check 3 (basis comparison):**
   - Put the public 30–40% promise next to the internal-plan statement.
   - For each, record metric, denominator, business perimeter, baseline, period and deadline.
   - Output: "Different basis disclosed", plus the CFO action: ask the synergy owner for the bridge from public baseline → internal plan → realised saving.
3. **13–14 Oct, check 1 (integration-cost reconciliation):** mostly researched already (section 4). Remaining:
   - check the audited notes to the accounts in the FY24/FY25 reports for integration cost
   - settle whether TSA costs are inside ₹1,106 cr
   - find the printed page for FY23 PDF p.109
   - build the period-and-scope table with assurance labels
   - if it can't be completed, show "Cannot reconcile the complete comparable cost from the reviewed sources"
4. **15 Oct, check 2 (bounded):** cards outstanding only, from RBI monthly tables (Axis, Citi, all banks around the closing date). Two-hour gate to confirm the definitions match. If they don't, publish "Not comparable on the available definitions" and stop. Never test the spend-share % with card counts.
5. **16 Oct:** freeze scope, recheck every number and link, rehearse with someone who doesn't know the deal.
6. **17 Oct:** fix only demo blockers; 3-minute demo plus a backup recording.
- **Deferred:**
  - AI "silence timeline" (check 4)
  - AI extraction build
  - goodwill / purchase-price allocation
  - bank-specific line-item map (`docs/forensic-extraction.md` is written for non-banks)
  - a fourth promise (the ₹2,000 cr integration budget as a tracked promise)
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

**Honest gap against it:** 1 deal, not 4–5, and no AI extraction yet. The scope was cut deliberately on 10 Oct (decision log rows 24–25). Say so plainly if asked.

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

## 10. Private context

`private/` holds working style, goals, outreach contacts (send the demo to the Accenture contact only once it works) and calendar constraints. Not in the public repo.
