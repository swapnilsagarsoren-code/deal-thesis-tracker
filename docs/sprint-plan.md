# Sprint plan — GrowthX Build Sprint, 2–17 Oct 2026

## Capacity

- 16 calendar days, minus Oct 10 = **~14 working days**
- Part-time alongside MBA coursework: realistic **3–5 hours/day**
- Builder: beginner Python, AI-assisted coding

## Scope

**Must ship**
1. 4–5 curated deals on the fixed test-plan template
2. Extract-then-compute pipeline working end-to-end on ≥2 deals (with citations)
3. Forensic Extraction line-item list applied to those 2 deals

**Roadmap slide only (do not build)**
- Live search via Linkup
- Forensic Scorecard
- Verdict history
- Payments

**Only if everything else is done:** methodology page.

## Time estimate

| Work | Days |
|---|---|
| Deal curation + collecting source filings | 2 |
| Extraction pipeline (prompt, citations, testing on real filings) | 3–4 |
| Threshold-check code + writing pre-registered thresholds | 1–2 |
| Wire mockup UI to real data | 2 |
| Testing, bug fixing, buffer | 2–3 |
| **Total** | **10–13** |

**Read:** fits, with almost no slack. If behind by ~Oct 8, drop to 4 deals. **Never** cut the extraction pipeline to make room — it is the only part that proves the AI is doing real work.

## Suggested day plan

| Dates | Focus |
|---|---|
| Before Oct 2 | Confirm GrowthX rules; write forensic line items; shortlist deals |
| Oct 2–3 | Collect filings; write pre-registered thresholds |
| Oct 4–7 | Extraction pipeline on deal #1 and #2 |
| Oct 8–9 | Threshold code; checkpoint — cut to 4 deals if behind |
| Oct 10 | Unavailable |
| Oct 11–13 | Remaining deals (lighter touch); wire UI |
| Oct 14–15 | Test, fix |
| Oct 16–17 | Buffer, demo recording, methodology page if time |

## Success check on Oct 17

- [ ] A stranger can open the site and see 4–5 deals with verdicts
- [ ] For 2 deals, clicking a verdict shows the extracted number, its source quote, the threshold, and the rule that produced the verdict
- [ ] Thresholds are dated earlier than the extraction run
- [ ] Nothing on the site claims live search works
