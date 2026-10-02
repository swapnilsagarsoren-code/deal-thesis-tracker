# Decision log

| # | Decision | Reason |
|---|---|---|
| 1 | India-focused tracker, not Capital One–Discover | Local relevance, fits Indian audience; C1–Discover kept as one benchmark card at most |
| 2 | Fixed test-plan template for every deal | Comparable verdicts; forces specific, checkable claims |
| 3 | Pre-register thresholds before outcomes | Prevents hindsight bias; audit-integrity principle; a real differentiator vs AI search |
| 4 | Extract-then-compute | LLM does what it is good at (reading documents); code does the pass/fail so verdicts are reproducible |
| 5 | Label non-numeric judgments "lower confidence" | Honesty about where AI judgment is fuzzy |
| 6 | Only deals with promises in acquirer's own public sources | Claims must be verifiable; avoids arguing about media paraphrases |
| 7 | Reviewed vs auto-generated tags | Users must see which results a human checked |
| 8 | Keep an honest "Unverifiable" result | Better than forcing a confident wrong answer |
| 9 | Linkup over Exa for search (post-sprint) | Exa grant application went unanswered; Linkup fits company/business search. Note: an independent benchmark had Exa slightly ahead on accuracy and speed, so this is a practical choice, not a quality one |
| 10 | Avoid Linkup "deep" search | ~10x cost and slow |
| 11 | No payments in sprint | API cost is near zero at this scale; building billing steals days from core work |
| 12 | Forensic Extraction in sprint, Forensic Scorecard deferred | Extraction is mostly a lookup table + prompt and is the unique audit angle; scorecard is another layer on an unfinished system |
| 13 | Live search deferred to post-sprint | Second independent integration; can't guarantee it works live in a demo |
| 14 | Scope cut to 4–5 deals, ≥2 full pipeline | Realistic for ~14 part-time days at beginner coding level |
| 15 | Stayed with the tracker over alternative ideas (forensic index, model auditor, M&A index) | No concrete flaw in the tracker was found; switching would restart planning with the sprint days away |
| 16 | Mockup as plain HTML, not Design-canvas artifact | Canvas format kept breaking interactivity |
| 17 | Axis–Citi replaces HDFC–HDFC Bank (28 Sep) | HDFC's thesis was mostly qualitative — no hard number to test |
| 18 | Renamed to "M&A Value Realization Tracker (India)" and framed as a CFO-office tool (2 Oct) | "Value realization" is the term finance and consulting teams use; tracking post-deal synergies is a CFO-office job, so the framing matches the work honestly |
| 19 | Value-driver tree built on financial-statement lines (2 Oct) | Tracing a promise to the line where it should show up in the accounts is the core finance skill to demonstrate; reuses the forensic line-item list |
| 20 | Drivers without a company target shown as "No target committed", never given an invented target (2 Oct) | Inventing targets breaks pre-registration and the company-sources rule; the gap is itself a finding |
| 21 | Scope cut to 4 deals; Axis–Citi and RateGain–Sojern get the full pipeline (2 Oct) | Pays for the value-driver tree; these two have the cleanest dated numbers |
| 22 | Dropped AI "management action" suggestions and generalising to "transformation programs" (2 Oct) | Outsider speculation a partner can't verify; internal transformation targets are not publicly disclosed, so M&A is the only area with public data |
| 23 | Never call it a "platform" or say it "continuously tracks" | It is a working prototype; overclaiming fails in interviews |
