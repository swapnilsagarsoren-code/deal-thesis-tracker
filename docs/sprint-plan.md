# Sprint plan — GrowthX Build Sprint, 2–17 Oct 2026

## Capacity

- 16 calendar days, minus Oct 10 = **~14 working days**
- Part-time alongside MBA coursework: realistic **3–5 hours/day**
- Builder: beginner Python, AI-assisted coding

## Scope

**Must ship**
1. 4 curated deals on the fixed test-plan template
2. Value-driver tree per deal, built on financial-statement lines
3. Extract-then-compute pipeline working end-to-end on **Axis–Citi** and **RateGain–Sojern** (with citations)
4. CFO value-realization summary table (status from code)

**Roadmap slide only (do not build)**
- Live search via Linkup
- Forensic Scorecard
- Status history
- Management-action hypotheses
- Payments

**Only if everything else is done:** methodology page; 3-bullet AI summary.

## Time estimate

| Work | Days |
|---|---|
| Forensic line-item list + collecting source filings | 2 |
| Value-driver trees + pre-registered thresholds (4 deals) | 1 |
| Extraction pipeline (prompt, citations, testing on real filings) | 3–4 |
| Threshold-check code | 1 |
| Screens: tree + summary table, wired to real data | 2 |
| Testing, bug fixing, buffer | 2–3 |
| **Total** | **11–13** |

**Read:** fits, with little slack. The value-driver tree was paid for by cutting from 5 deals to 4. If behind by ~Oct 8, drop the other 2 deals to "targets and tree only" (no extraction). **Never** cut the extraction pipeline on Axis–Citi and RateGain–Sojern — it is the only part that proves the AI does real work.

## Suggested day plan

| Dates | Focus |
|---|---|
| Oct 2–3 | Confirm GrowthX rules; finalise forensic line items; pick the other 2 deals; collect filings |
| Oct 4 | Value-driver trees; write pre-registered thresholds (dated) |
| Oct 5–8 | Extraction pipeline on Axis–Citi and RateGain–Sojern |
| Oct 9 | Threshold code; checkpoint — cut the other 2 deals to tree-only if behind |
| Oct 10 | Unavailable |
| Oct 11–13 | Tree + summary screens; wire real data |
| Oct 14–15 | Test, fix |
| Oct 16–17 | Buffer, demo recording, methodology page if time |

## Success check on Oct 17

- [ ] A stranger can open the site and see 4 deals, each with a value-driver tree
- [ ] For Axis–Citi and RateGain–Sojern, clicking a target shows the extracted number, its source quote, the threshold and the rule that produced the status
- [ ] Thresholds are dated earlier than the extraction run
- [ ] No invented targets or numbers anywhere; nothing claims live search works
