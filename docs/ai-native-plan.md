# AI-native build plan (drafted 21 Oct 2026)

**Goal:** the live site shows AI doing the part that stops this from existing today: reading new filings. The loop is: **AI finds → code checks it word for word → Swapnil accepts → fixed rules decide.** The same loop runs again every quarter and works on a second deal.

**Budget:** about 3 hours a day, 12 working days (about 36 hours). Each phase ends in a check that passes or fails. Back up prod before each release (README "Deploy").

**What does not change:** `shared/statusRules.ts` decides every status. AI never writes a confirmed field. Nothing unreviewed is shown to the public.

---

## Phase A: finder on prod (days 1–2, ~6 h)

The finder works but is locked to dev in five places.

1. Remove the hard-coded dev URL checks:
   - `convex/finder.ts:15` (`devOnly`)
   - `shared/finderLimits.ts:8`
   - `convex/finderReviewRead.ts:12`
   - `shared/finderPageUpload.ts:3-4`

   `FINDER_ENABLED` + `EDIT_PASSCODE` + the daily limits are the guard. Leave `portfolioMigration.ts` dev-only.
2. **Give each document its own index version.** Today `PAGE_INDEX_VERSION` hashes all 11 documents together (`scripts/build-page-index.mjs`). Adding one transcript would invalidate every page and every cached run. Change it to `INDEX_VERSIONS[docId]`, and use `docId+version` in `sourcePage` and in the finder cache key.
3. Add an optional `docIds` argument to `runFinder` (`shared/finderRunner.ts`), so a re-check searches only the new document. Today it searches all 11, about 12 calls a run.
4. Set `ANTHROPIC_API_KEY` and `FINDER_ENABLED=true` on prod. Upload the 1,994 pages to prod with `upload-pages-dev.mjs`, generalised to take a deployment name. Run the finder once per promise.

**Check:** `npm test` passes 250 of 250 and the new tests pass. Prod `finderRun` has 4 completed rows. `finderUsage.tokens` is under the 600k daily limit. Logged-out page diff is empty: nothing is visible until it's reviewed.

## Phase B: public review trail and accept/reject queue (days 3–4, ~6 h)

1. **Queue:** at `?edit=1#review`, one list of every `found` quote across all promises that has no decision yet. Show the quote, the page link, the code check result and any injection flags, with Accept and Reject buttons. Reuse `finder.review` and `EvidenceFinderView`; this is a new list view, not a new backend.
2. **Accept → draft evidence:** add an internal mutation that copies an accepted quote, URL and page into the promise's `resultQuote/resultUrl/resultPage`. It leaves `resultConfirmed=false`, so the status can't change until you confirm in the existing form. It uses the same revision check as `deal.saveAuthorized`.
3. **Public trail:** set `SHOW_FINDER_ON_PUBLIC_CARDS=true` (`shared/credibilityContent.ts`), but show only accepted finds. Each one is labelled "Found by AI (Claude Haiku) · quote checked word for word by code · accepted by Swapnil Sagar Soren, <date>". Also show a count line: "AI searched 11 documents, 1,994 pages; 3 passages suggested, 1 accepted, 2 rejected (off-question)."
4. **Method page:** publish the eval numbers from HANDOFF:
   - Practice set: 7, 8 and 7 out of 10.
   - Held-out set: found 4 of 6, and correctly found no figure in 3 of 3.
   - Real but off-question passages that a person had to reject, with the ₹1,106 cr example.
   - Cost.

**Check:** at 390px and 1440px, logged out, no unaccepted quote appears in the DOM. One accept → the draft is filled → the status is unchanged until you confirm. Run `check-live.mjs` and expect PASS.

## Phase C: scheduled quarterly re-check (days 5–7, ~9 h)

This turns your 20 Oct manual Q2 FY27 read (watching two recordings and searching the captions) into a scheduled job.

1. **`sources.json`:** one watch-list entry per deal, giving the results and transcript pages to check and the PDF link patterns. For Axis: the quarterly-results page and the call-transcript archive (both URLs are in `integration-cost-evidence.md`).
2. **`scripts/recheck.mjs`:** fetch the pages, find PDF links not yet indexed, download them, then run `pdftotext` with the same code as `build-page-index.mjs`. Then:
   - upload the pages;
   - call `finderActions.run` with `docIds:[newDoc]` for each promise of that deal;
   - write a `recheckLog` row: date, document, pages, per-promise outcome (found, none or rejected), and tokens.
3. **Where it runs:** a scheduled GitHub Actions job in the app repo, weekly in results months (Jan, Apr, Jul, Oct), plus a manual trigger. Shortcut: Convex actions can't run Poppler, so the PDF extraction runs in Actions. Move it into a Node action (e.g. `unpdf`) only if Actions becomes a problem.
4. **Fallback:** if Axis blocks automated downloads, add a "Add document URL" field in edit mode that runs the same script path.
5. **On the site:** add a "Last checked" line per deal, e.g. "21 Oct 2026 · Q2 FY27 presentation, 79 pages · no Citi-specific update found · next check Jan 2027". A "nothing found" result is a finding, so show it.

**Check:** run it by hand on the Q2 FY27 investor presentation. The expected result is "none" for all 4 promises, which matches your manual read on 20 Oct. That's a free held-out eval case: write it down before running. Then run it a second time and confirm nothing is re-uploaded or re-billed.

## Phase D: second deal, RateGain × Sojern (days 8–11, ~12 h)

Why this deal: it's on the shortlist, with targets at the Mar-2026 exit and FY26 results already out. Its Q2 FY27 results arrive in early November, so the re-check from Phase C fires on a live event during this build. It also contrasts with Axis: a company that keeps disclosing versus one that stopped.

1. **Selection rule first** (13 Oct note in `deals.md`): write the rule as a decision-log row, and say that the FY26 outcome was already seen.
2. **Make the code work for more than one deal:**
   - Add `dealId` to `deal` and `additionalPromise`.
   - Replace the literal unions in `convex/schema.ts`, `convex/finderValidators.ts:2` and `shared/finderDocuments.ts` with a registry in `shared/deals.ts`: deal → promises → documents.
   - Migrate the 4 Axis rows with an idempotent script that has a dry run (the pattern in `seed-prod.mjs`).
   - Route `#deal/axis-citi` and `#deal/rategain-sojern`, with a 2-row index page.
3. **AI-drafted promises:** a new prompt over the deal call and deck: "list measurable promises; return exact quotes only." Code checks each quote word for word on its page, as in `finderRetry`. You confirm the target, unit and deadline in the existing form, so the AI proposes the promise list and a person decides it. Show "Promises proposed by AI: N; kept by reviewer: M" on the deal page.
4. **Rule gap (decide before building):** "Sojern EBITDA margin 16.5–17.5%" is a range. Today only the approved 30–40% Axis range is handled (`statusRules.ts`, `costRange`), and the README says other ranges stay Unverifiable. Choose one:
   - **(a)** a general range rule: lower bound = Delivered, between 0 and the bound = Partial;
   - **(b)** approve this one range as a second exception;
   - **(c)** leave it Unverifiable.
5. Run the finder on the RateGain filings for each promise. Review the results.

**Check:** both deals render, and the Axis page output is identical to before the migration (snapshot diff, ignoring `asOf`). RateGain has at least 3 promises with rule-set statuses. Record the finder's score on RateGain as a **held-out** score, because the finder was never tuned on this deal. That number is the real proof that it generalises.

## Phase E: release and story (day 12, ~3 h)

- Back up prod → deploy → `check-live.mjs` → screenshots.
- Update the README, HANDOFF §2–3 and the GitHub About line: "AI finds, code checks, a person accepts, fixed rules decide. Re-checked every quarter."
- Kill or redirect the old Vercel mockup ("6 Deals tracked").
- One-line story: *"Every quarter it reads the new filings for each tracked deal, proposes cited evidence, and a person accepts or rejects it; the status follows fixed rules. Held-out accuracy on a deal it never saw: X/Y."*

---

## Decisions needed from Swapnil

1. Second deal: RateGain × Sojern (recommended), or pick by a new rule first.
2. The range rule for the RateGain margin targets: (a), (b) or (c) in Phase D.
3. Public visibility: accepted finds plus counts only (recommended), or also show rejected quotes with the reason.
4. Monthly AI spend cap. Re-checks cost very little; the whole 11 Oct live test cost about ₹10. Suggest keeping the current daily limits and the $5 prepaid credit with auto-reload off.

## Risks

- **Axis site blocks downloads:** the manual URL fallback in Phase C.
- **New documents may contain prompt-injection text:** `scanInstructions` flags already run per page, so show the flags in the queue.
- **Off-question but real quotes** (seen 4 times in testing): this is why every find is accepted by a person, and the public count line makes that visible.
- **Prod data:** migrations only through dry-run scripts, with a backup before each phase release.
