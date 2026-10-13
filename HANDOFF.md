# HANDOFF — read this first

For any AI model or person continuing this project. Last updated: **12 Oct 2026, ~13:20 IST**.

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
- **Portfolio page live:** https://hq.swapnilsoren.com/builds/deal-thesis-tracker (repo `swapnilhq`). Since 11 Oct evening it carries the long-form build write-up (key points, 8 sections, sources), status "In progress", and a note that the full tracker goes live on 16 Oct. No "Read the method" button until prod has the method page. On 16 Oct: remove the note, add the method button, set status "Live" (the `drafts/` copy holds the final frontmatter).
- **Old mockup:** https://deal-thesis-tracker.vercel.app. Superseded; not linked anywhere new.

- **11 Oct, on dev only (not on prod):**
  - Two more promises, both **Unverifiable** via the unchanged status rules: integration budget ("Rs 2,000 crores pre-tax … over 18 months period", call p.8) and EPS/ROE accretion ("EPS & ROE accretive in CY24", deck p.5).
  - Reporting-basis panel for cost savings: overall **Different basis disclosed** (6 rows: metric, size, baseline, perimeter, period, measured-against), missing bridge, documents searched, blank reviewer fields.
  - "Considered — no target committed" list and "Not promises — closing-date pro forma figures" list.
  - Annual-report links FY23–FY26 verified (HTTP 200, page counts 376/466/504/520, cited text on cited page). Offsets: FY23 printed = PDF − 2; FY24/FY25 printed = PDF; FY26 printed = PDF + 1.
  - Stored in two NEW tables (`additionalPromise`, `reportingReview`); `deal` table and `shared/statusRules.ts` untouched. New save functions need the passcode and are locked to dev — that lock must be changed deliberately on prod release day.
  - Gotcha: a daily cron rewrites `assessment.asOf` on the `deal` rows at midnight IST. Snapshot comparisons must ignore that one field.
  - Integration-cost panel: ₹1,323 crore "Disclosed, months 1–13 of 18", no variance, exclusions collapsed.
  - **Phase 4.0–4.7 polish (deployed and seeded on dev, 11 Oct evening):**
    - Top summary table "What was promised vs what was reported" (4 rows, status badges from the rules), then the one-line finding: "1 delivered (management-reported), 3 cannot be verified from Axis's public sources…".
    - Integration threshold now "complete the full integration" (exact substring), with "'Seamlessly': not tested" shown; promise quote trimmed of "And number five,". Status still Delivered.
    - Integration-budget card: target "~₹2,000 crore pre-tax over 18 months (an estimate, not a cap)", deadline 31 Aug 2024, reported "₹1,323 crore for months 1–13; months 14–18 not disclosed", measured as of 31 Mar 2024, analyst refusal cited (Q1FY25 p.18). Status Unverifiable.
    - EPS/ROE card: "No reviewed document (annual reports FY24–FY26, Q4FY24–Q4FY25 calls) states whether the acquisition was EPS or ROE accretive…". Verified by text search: every "accretion" hit in those documents is about CET1 capital or deposits.
    - Auditor table shown once; empty finder timelines replaced by "Evidence finder: not run for this promise."; one date format.
    - Key findings (5 bullets), 8-item menu with click lock and scroll tracking, iOS-style section picker on phones, stacked tables on phones (Phases 4.5–4.7).
  - **Decision (11 Oct): prod release waits until 16 Oct**, together with the final recheck of numbers and links. Release = snapshot prod → lift the dev-only lock on the new save/seed functions (passcode stays) → deploy → seed prod → snapshot → check live page at 390px/1440px. An early release on 11 Oct was considered and declined (too many steps); the Phase 5.0 release prompt drafted in chat on 11 Oct is the checklist for 16 Oct.
  - **Decision (11 Oct): reviewer = Swapnil for now**; fill name/date on prod via `?edit=1` after release. Swap in a second reviewer if one becomes available.
- **12 Oct (dev only): Phase 4.8** hides the finder/timeline section on public cards behind a flag (edit mode unchanged), adds one cited sentence to the integration-budget card ("The ~₹2,000 crore estimate is last repeated in the FY23 annual report (PDF p. 109). No later document reviewed refers to it or compares spending against it.") and syncs the methodology page §4/§7 text.

**Honest weakness (Swapnil's own view, agreed):** so far it compares management's words with management's words. The fix is to test claims against evidence management doesn't control in the same way: annual-report figures, regulator data, and what management declined to disclose.

## 4. Findings so far (verified against the PDFs)

- **Cost savings: different basis disclosed.** The public promise was 30–40% of CY2020 Citi opex over 2 years. The later success claim is against an internal six-quarter Board plan. No bridge between the two is in the reviewed sources.
- **Integration cost** (full table: [`docs/integration-cost-evidence.md`](docs/integration-cost-evidence.md)):
  - Estimate: about ₹2,000 cr pre-tax over 18 months (1 Mar 2023 call p.8; repeated in FY23 AR PDF p.109, covering "transitional costs and integration expenses").
  - Disclosed: FY23 ₹217 cr (FY23 AR PDF p.109) and FY24 ₹1,106 cr (FY24 AR printed p.212 = PDF p.212, MD&A). That's ₹1,323 cr for 13 of 18 months.
  - Apr–Aug 2024 not disclosed. FY25 AR (PDF p.238) says only that integration costs fell.
  - Both figures are management narrative in the annual report, not confirmed as audited notes. Deal deck p.15 note 3 says the integration estimate "includes transition services agreement expenses to be paid to Citigroup" (already in docs/integration-cost-evidence.md), so the estimate includes TSA; whether ₹1,106 cr does is not stated outright.
  - **The estimate is last repeated in the FY23 AR (PDF p.109).** Text search of all 11 documents (first done 11 Oct, re-run 12 Oct): no later document restates ~₹2,000 cr or compares actual spend against it. Later ₹2,000 cr / ₹1,500 cr hits (FY26 p.107, 147, 209, 211) are a fund launch and bonds, not the integration estimate. The completion announcement and the Q4FY25 call don't mention integration costs.
- **Declined to disclose:**
  - Q1FY25 call, PDF p.18: an analyst asked for integration costs and retention ratios.
  - CFO's answer included "we don't share specific numbers", followed by a claim that every acquisition metric was "met or exceeded".
- **Analysts asked for numbers repeatedly** (call transcripts): attrition (closing call p.5 Mahrukh Adajania; p.6–7 Adarsh Parasrampuria; corrected 12 Oct, was "p.4"), CY24 accretion timing (p.11, Rahul Jain), retention and integration costs (Q1FY25 p.18, Anand Dama), synergy quantification (Q4FY25 p.13, Anand Swaminathan). No retention figure and no rupee/% realised-synergy figure appears in the FY24–FY26 annual reports or later calls (text search).
- **Synergy reporting stops being separable:** Q4FY25 call p.13 (Subrat Mohanty): "we no longer separate the Citi and the Axis book and monitor their progress". Management's own reason why accretion, retention and synergy can't be checked from outside after LD2.
- **No synergy number in any annual report FY23–FY26.** Closest: FY24 AR p.232, "70 synergy initiatives across cross-sell, deepening, sales productivity and cost rationalization" (deal call said "60+").
- **Accretion wording differs across deal-time documents:** deck p.5 "accretive in CY24" vs FY23 AR printed p.10 (PDF p.12) "accretive at the end of calendar year 2024". Different basis disclosed. No later report mentions accretion.
- **Axis never committed to publish integration numbers:** 1 Mar 2023 call p.7, "Right now we have not decided what we'll share and how much we'll share."
- **Market-share claim, corrected:** the deal deck's 11.4% → 16.2% is a share of credit-card *spends/advances* (p.14 footnote), not card counts. The 9.4M + 1.8M = 11.2M card figure is on p.10, with a consent cutoff of 20 Feb 2023.
- **External context (11 Oct research, for the story only; not evidence about Axis):** BCG (Nov 2025, 175 deals >$1bn, 2019–23) found acquirers that disclosed actual synergy realisation had ~6% higher relative TSR over the next two years. Never claim this effect applies to Axis.

**Interview line (defensible):** "Axis disclosed ₹1,323 crore of integration cost against a ~₹2,000 crore estimate, covering 13 of 18 months; when an analyst asked for the numbers, management said it doesn't share specific numbers, while saying every target was met. And its cost-saving success was measured against an internal plan, not the public 30–40% promise. I built a cited tracker that makes these evidence gaps visible without calling them failures."

**Timeline line (demo and interviews):** "Axis's ~₹2,000 crore integration estimate was last repeated in its FY23 annual report; no later report refers to it or compares spending against it. I checked that page by page across all 11 documents. I also built an AI version that labels each document, but in testing it didn't return that FY23 sentence, which would have overstated the gap, so it isn't on the public page."

## 5. Locked decisions (reasons in [`docs/decision-log.md`](docs/decision-log.md))

1. **Sprint scope (changed 10 Oct):** Axis–Citi in depth only. No new deals before 17 Oct.
2. Status is decided by deterministic code, never by an LLM. `shared/statusRules.ts` is not changed without an explicit decision.
3. Only targets the company itself stated publicly. Never invent a target; a gap is shown as a gap.
4. **Honest sequence:** promises are recorded exactly as stated and then checked. This was retrospective (deal picked and results read the same day), so don't claim "fixed before seeing results".
5. **Wording:** "public-disclosure reconciliation", not "forensic proof". "Different basis disclosed / Comparable / Insufficient detail", not "moved goalposts". "Unverifiable" or "cannot reconcile", never "failed", when data is missing.
6. Never call it a "platform" or say it "continuously tracks".
7. Silence is not failure. Not disclosing ≠ not delivering.
8. **Finder frozen (11 Oct, decision log row 36):** no further finder tuning before 17 Oct.
9. **Disclosure timeline (row 31): built on dev, not shown publicly (12 Oct, row 37).** Per promise and per document, code labels each source: Restates target / Reports a figure / Mentions, no figure / Declined when asked (AI-flagged, person confirms) / No passage found by AI search. The headline "Target last referred to: <document>" is computed by code. It never sets a status and never says "silent". It isn't run for the demo: in live tests the finder did not return the FY23 AR p.109 restatement, which would move the headline to 1 Mar 2023 and overstate the gap. A reviewer can only accept or reject a finding, so a missed passage can't be fixed by hand. The public budget card shows the same finding from a hand text search instead (§4). Note for any future run: code labels deck p.15 "Restates target" (₹1,500 cr post-tax is half of the same estimate; integration-cost-evidence.md calls the 2-financial-year period a different basis) and Q4FY24 p.9 "Reports a figure" (~11% of Q4 YoY cost growth, a percentage, not rupees).
10. **CFO close-out check approved by Swapnil (12 Oct, row 38), static content only.** Six items a CFO's office needs to close out the deal vs what Axis disclosed, ranked by the number of distinct analysts who asked on the 5 calls reviewed (integration cost 4, retention 3, EPS/ROE 3, synergies 2, migration 0, TSA exit 0). Coverage labels: Disclosed / Partly disclosed / Different basis disclosed / Not disclosed / Declined when asked; each line Observed or Inferred. No scores, no weights, no AI. Headline (corrected 12 Oct after recount; "asked about most are the least disclosed" was not supported: integration cost, the most asked, is Partly disclosed, and TSA exit has 0 asks and is Not disclosed): "1 of 6 items fully disclosed. None of the 4 items analysts asked about is fully disclosed." Counts shown as deal-day vs later (e.g. "4 (3 on deal day, 1 later)"); one question can count under two items (the 3 EPS/ROE asks are the same 3 questions counted under integration cost); 7 analysts asked, 5 of them on the 1 Mar 2023 call; Mahrukh Adajania (call p.5) counted under retention as Inferred. FY24 AR p.212 calls TSA costs a kind of integration expense ("the transitional services agreement and other integration expenses") just before ₹1,106 cr, so ₹1,106 cr very likely includes TSA (Inferred, not stated). Build prompt: Phase 4.9 in chat 12 Oct. Built as Phase 4.9 after 4.8. Goodwill/PPA stays out of scope.

## 6. Guardrails for anyone changing the app

- **Dev and prod have separate databases.** A data change on dev doesn't reach prod. Use a one-off, idempotent internal migration: snapshot prod before, migrate, snapshot after. Or edit on prod through `?edit=1` with the passcode.
- **The passcode** is never pasted into chat, code, logs or commits. **The Anthropic API key** is entered only through hidden `read -s` prompts.
- **Loading or fetch failure must never show a verdict.**
- **`#page=N` uses the PDF page index.** Annual reports can differ from printed pages, so show "printed p. X (PDF p. Y)" when they differ. Keep the page label visible as text, because iOS Safari ignores `#page`.
- **No invented numbers.** Every number traces to a page. Label each number's assurance level: audited note, annual-report narrative, or call transcript.
- **Ask Codex for raw test output, not summaries.** Verify prod with the read-only Convex connection after every ship.
- **App code:** ~100 files of Phase 3–4.7 work were uncommitted as of 11 Oct night; make a local commit before any release (no push).

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
7. **16 Oct:** freeze scope, recheck every number and link, rehearse with someone who doesn't know the deal, prod release, then switch the HQ page to final.
8. **17 Oct:** fix only demo blockers; backup recording.
- **Deferred:**
  - Showing the AI disclosure timeline publicly: after 17 Oct, and only once a test run returns the FY23 AR p.109 restatement (finder frozen, row 36; hidden, row 37).
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
| Source PDFs (Axis reports, transcripts, decks) | `~/Downloads/growthxbuilddocs`; also uploaded to the claude.ai Project "M&A Tracker" (which is full); page text for all 11 documents in `~/build-sprint-app/data/pages/*.jsonl` |
| HQ portfolio site | `~/Downloads/Work/swapnilhq` → GitHub `swapnilhq` (private) → Vercel → hq.swapnilsoren.com |
| Career notes | `~/Downloads/Work/Career-OS/master-repository/experience-bank/ai-products.md` (Product 4) |
| Private notes, prod snapshot | `private/` in this folder (gitignored) |
| Backup of pre-rebase local history | branch `backup-before-rebase-10oct` in this repo |
| Ship check (11 Oct) | claude.ai Project "M&A Tracker" (private) |

## 10. Private context

`private/` holds working style, goals, outreach contacts (send the demo to the Accenture contact only once it works) and calendar constraints. Not in the public repo.

## Release checklist additions (11 Oct)
- **13 Oct status.** Phase 4.11 committed a1d568d; Phase 4.12 (methodology RBI exception + §5 close-out/regulator items + sources; close-out employee-retention line, FY23 AR printed p.31 / PDF p.33 and Q4FY25 p.13) committed 9422c7f; Phase 4.13 (Key findings date wording) applied by Codex, commit pending. docs/methodology.md and the project copy synced to the site 13 Oct.
- **Deep check 13 Oct (parts 1+2): no blockers.** 25 annual-report figures re-verified on an independent extraction; negative claims hold (no EPS/ROE accretion statement FY24–FY26, ~₹2,000 cr only in FY23, no rupee synergy figure). Prod: Integration Delivered rev 3, Cost savings Unverifiable rev 1, additionalPromise/reportingReview empty (release adds them). Dev page at 390px and 1440px: no horizontal scroll, 9-item menu/picker, no console errors, no stuck loading, taps ≥44px. HQ claim "key audit matter only in FY23" holds (FY23 standalone AR PDF p.212). Fixed from it: methodology self-contradiction, employee-retention disclosure, "a year after" → "nine months after" (Q4FY25 call 25 Apr 2025 vs final migration 14 Jul 2024).
- **Open from deep check:** (a) HQ page says the full version "with … the AI evidence finder … goes live on 16 October" — the finder stays off; fix in the 16 Oct HQ switch (proposed: "…with all four promises, the CFO close-out check, the RBI data check, the auditor check and the method page, goes live on 16 October 2026. The AI evidence finder stays switched off on the live site."). (b) Note: Key-findings source strip on phones scrolls sideways (scrollbar visible). (c) Note: FY23 AR p.119 "12.1 million cards in force" matches RBI Mar 2023 12,127,172; FY23 AR p.20 "ROE accretive post integration" is a third accretion wording. (d) Source links last opened 10–11 Oct; re-test 16 Oct in the built-in browser (device shell cannot reach axis.bank.in).
- **Next:** 16 Oct 09:00 IST deep-check re-run (scheduled), then Phase 5.0 release (appendix). Docs repo (this folder) has uncommitted changes since 12 Oct; it is public on GitHub, so push only with Swapnil's OK.
- **Deep debugging check (added 12 Oct at Swapnil's request; run 13 Oct 10:00 IST, re-run 16 Oct before release).** Read-only, findings ranked blocker / fix before 16 Oct / note: (1) every quote, number and page on the site re-verified against data/pages/*.jsonl and RBI pages; (2) each status recomputed from stored evidence with statusRules.ts; (3) prod vs dev read-only snapshot diff; (4) every source link 200 with cited text on the cited page; (5) site-wide wording audit (no platform / forensic proof / continuously tracks / failed / silent / goalposts / red flag); (6) test honesty: Mac fail 0, no skipped or mocked tests, Chrome actually launches; (7) dev page at 390px and 1440px incl. loading and failure states (no verdict), 9-item menu, no horizontal scroll, 44px taps; (8) no secrets in repo or history, finder off and dev-locked on prod; (9) HANDOFF, decision log, methodology, HQ page and interview lines agree; (10) clean tree, no lock files, prod rollback hash recorded. Fixes go to Codex as one prompt ending with the one-line test→deploy→commit command.
- Phase 4.9.1 committed 435022d (first full Mac run: 228/228 incl. real-browser test). Phase 4.10 RBI card-transfer panel committed 683778d (row 39). Phase 4.11 pre-release fixes prompt issued 12 Oct.
- **Tests count only on Swapnil's Mac (added 12 Oct).** Codex's sandbox cannot launch Chrome, so the real-browser test (tests/navigationBrowser.test.ts) silently failed to launch in every Codex run from at least Phase 4.7 to 4.9 (artifacts/phase4-7*, phase4-8 test-output.txt). First real run on the Mac (12 Oct, after 4.9) found a back-to-contents bug: replaceState(pathname+search) throws on an about:blank origin. Visitor impact: none on the real https site. Fixed in Phase 4.9.1 (href without hash). Release gate: `npm test 2>&1 | grep -E "(tests|pass|fail) [0-9]+"` on the Mac must show fail 0 (Mac Node prints 'ℹ tests', not '# tests').
- Phase 4.8 committed cdf7d18; Phase 4.9 (CFO close-out, row 38) committed 5351fb9 and deployed to dev 12 Oct. Claude corrected one 4.9 citation before deploy: synergy 'internal Board plan' line now cites management's answer (Q4FY25 p.13 "synergy benefits that we internally presented to the Board", "we came a bit ahead of the plan"), not the analyst's question.
- Small 4.9 risk to fix before 16 Oct: CloseOut Citation throws if a source URL is missing (whole page would fail to render); show "source unavailable" instead.
- Phase 3.2 changed every passcode-protected function (including edit-save) from mutation/query to action: retest edit mode, reviewer save and finder run on dev before prod.
- Passcode lockout is global per India hour: 10 wrong guesses lock everyone (including you) for the rest of that hour.
- New save/seed/finder functions are locked to dev; lift deliberately on release day (save/seed only; finder stays dev-locked and off on prod).
- Fill "Reviewed by" (Swapnil, date) on the reporting-basis panel after release.
- Known small fix: FY25 section map keeps PDF 275–276 as MD&A (per contents page) though they hold the governance certificate; set MD&A end to 274 before release.
- Methodology text §4 and §7 updated 12 Oct in docs/methodology.md (how the AI is used, verified = words on the page, held-out 4/6, switched off on this site, timeline not shown; "a missing disclosure" instead of "silence"). Phase 4.8 copies it into the app's methodology page.

## AI evidence finder — status 11 Oct, 18:30 IST (deployed on dev; FINDER_ENABLED still off; not on prod)
- Built by Codex in phases 1 → 3.7; checked by Claude after each phase. Your Mac: 160/160 tests at Phase 3.6; statusRules and ledger values unchanged.
- What it does: per promise, searches 11 Axis documents (1,994 pages; uploaded to dev), Claude Haiku 5.5 returns verbatim quotes only; code verifies every quote word for word, labels each document, checks figures against a hand-verified ledger, hides anything unverified from the public until Swapnil accepts it, flags prompt-injection text, labels assurance by annual-report section.
- Live-test fixes on 11 Oct: `temperature` removed (rejected for this model); thinking disabled and output cap 1,000 (replies were being cut off); rarity-weighted search; page priority changed to "top 3 by relevance + 1 slot for MD&A/notes"; one exact-copy retry on a word-for-word reject.
- **Live scores (Haiku 5.5):** practice set (tuned on) 7/10, 8/10, 7/10; held-out set (frozen before running) 4/6 found and 3/3 correctly found no figure. Total live-test cost ≈ ₹10.
- **Key limit:** "verified" means only "these words are on that page". No invented quote or number passed in any run, but 4 verified passages were real yet off-question (e.g. ₹1,106 cr returned for a synergy question; an analyst's question quoted as a report). Hence: a person must accept every finding.
- **Disclosure timeline was tested only with scripted (stub) replies** (11/11 labels, `artifacts/finder/phase3-2-timeline-eval.txt`: "model accuracy not evaluated"). It has never been run live. In live runs the finder returned no FY23 restatement and its ₹217 cr quote failed the word-for-word check.
- Cost controls: kill switch (FINDER_ENABLED off by default), passcode + dev-only, 5 runs / 60 calls / 600k tokens per day, 12 calls per run, 2-min cooldown, $5 prepaid credit with auto-reload off.
- Run files: `~/build-sprint-app/artifacts/finder/live-eval-run*.json`.

## Appendix — Codex prompts kept for the next sessions (updated 12 Oct, 13:20 IST)

- **Phase 4.8:** done. Deployed to dev, committed cdf7d18.
- **Phase 4.9 (CFO close-out):** done. Built from the corrected prompt written in chat on 12 Oct (an older draft with the headline "The items analysts asked about most are the least disclosed" was dropped as unsupported). Deployed to dev, committed 5351fb9. Content in ~/build-sprint-app/shared/closeOutContent.ts.
- **Phase 4.9.1:** back-to-contents URL reset uses href without hash (fixes the real-browser test on about:blank). Applied by Codex 12 Oct; commit pending at time of writing.

### Phase 5.0 — prod release checklist for 16 Oct (drafted 11 Oct; updated 12 Oct for 4.8/4.9/4.9.1)
```
PHASE 5.0 — PROD RELEASE PREP. You PREPARE and CHECK; Swapnil runs the prod-writing commands. STOP at the two gates.
HARD RULES: no change to shared/statusRules.ts; never print/log/commit EDIT_PASSCODE or API keys; no push/merge; no git write commands (Swapnil commits); finder stays OFF on prod (do NOT lift the dev-only lock on convex/finder.ts, finderReviewRead.ts, shared/finderLimits.ts, finderPageUpload.ts; do not set FINDER_ENABLED or ANTHROPIC_API_KEY on prod; do not upload pages to prod). Existing prod statuses must not change: Integration = Delivered (rev 3), Cost savings = Unverifiable (rev 1) — STOP if any step would change them. Ignore assessment.asOf in snapshot comparisons.
STEP 1 Rollback point: confirm the tree is committed (Swapnil commits); record the hash currently live on prod (8c3fce7) as rollback target.
STEP 2 Fix: FY25 section map MD&A end page = 274 (+ test). Confirm Methodology.tsx has the 4.8 text. npm test + npm run build, RAW output. The real-browser test cannot run in your sandbox; say so. GATE: Swapnil's Mac must show fail 0 (`npm test 2>&1 | grep -E "(tests|pass|fail) [0-9]+"`).
STEP 3 Read-only full prod snapshot → artifacts/prod-snapshot-<date>-pre-release.json; print row counts and each deal promise's status + revision.
STEP 4 Lift dev-only lock on non-finder save/seed functions (additionalPromises.ts, reportingReview.ts, seedPromptA.ts, others) keeping the passcode check; list file:line of each change; add a test proving finder functions still refuse on non-dev.
STEP 5 scripts/seed-prompt-a-prod.mjs (flag --approved-prod-write; idempotent; refuses any status/revision change; prints dry-run diff table/row/field before → after). List every EXISTING prod field that will change. Rehearse twice on an exact copy of the prod snapshot (as rehearse-portfolio-migration.mjs); RAW output. (The CFO close-out section is static code: no seed rows.)
GATE 1 — STOP. Print commit + rollback hashes, RAW test/build output, rehearsal diff, lock changes, and the EXACT commands Swapnil runs in tab 2 (prod deploy incl. static hosting; seed dry run; seed write; passcode via hidden prompt only).
STEP 6 After "released": post-release snapshot; diff vs pre (statuses/revisions unchanged; new rows only in additionalPromise/reportingReview; only listed fields changed); scripts/check-live.mjs on https://doting-zebra-426.convex.site (4 promises, summary, CFO close-out section with headline "1 of 6 items fully disclosed. None of the 4 items analysts asked about is fully disclosed: …", key findings, 9 menu items, auditor table, methodology §4 new text, no stuck Loading, all source links 200). RAW output.
GATE 2 — STOP. Swapnil fills "Reviewed by" via ?edit=1.
```
After release: switch the HQ page (remove the 16 Oct note, add methodLink https://doting-zebra-426.convex.site/#methodology "Read the method", status "Live"), rotate the Anthropic key.
