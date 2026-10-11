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
| 24 | Sprint scope cut to Axis–Citi only, in depth; no new deals before 17 Oct (10 Oct) | One deal tested against reported figures beats four deals of quotes; 7 days left |
| 25 | AI extraction deferred; forensic checks (basis comparison, integration-cost reconciliation, bounded RBI card check) come first (10 Oct) | Testing claims against figures management doesn't control fixes the "words vs words" weakness; AI would only speed up quote-finding |
| 26 | Wording: "public-disclosure reconciliation", "Different basis disclosed", "Unverifiable / cannot reconcile", never "forensic proof", "moved goalposts" or "failed" (10 Oct) | Audited accounts are still management-prepared; you can show a basis mismatch, not intent; silence is not failure |
| 27 | Sequence described honestly as retrospective, not "fixed before seeing results" (10 Oct) | Deal picked and results read the same day; claiming outcome-blind pre-registration would fail scrutiny |
| 28 | Edit lock via server-side passcode; loading never shows a verdict; integration "Delivered" narrowed to "reported completion, seamlessness not tested" (10 Oct) | A public portfolio link must not be editable or show false verdicts; the evidence supports only the reported date |
| 29 | Market-share check limited to card counts; the 11.4%→16.2% spend-share figure is not tested with card data (10 Oct) | Different measures; mixing them would be an invalid comparison |
| 30 | AI evidence finder unlocked by Swapnil (11 Oct), Axis–Citi only, Anthropic Claude | "AI-native" requirement for the sprint and placement story. Narrow role: find passage + page or show what was searched; code verifies the quote word for word; a person confirms; fixed rules still decide status. Full auto-pull pipeline (NSE/BSE scraping, any company) stays deferred |
| 31 | Disclosure timeline unlocked by Swapnil (11 Oct): per promise, per document, code labels restates / reports a figure / mentions / declined (human-confirmed) / no passage found; headline 'target last referred to' | The verified finding that the ₹2,000 cr estimate is never referred to after FY23 is the strongest, judgment-free insight. AI returns only verbatim quotes; code assigns labels; never 'silent', never intent |
| 32 | Trust layer before any live AI run (11 Oct): verified-numbers ledger cross-check, unit/period number check, injection scanner, temperature 0, permanent attack tests | Blind tests showed Haiku resists injection but went quiet on a real passage when attacked; a planted fake fact would pass the word-for-word check. Code, not model luck, must catch it |
| 33 | Chart/image reading deferred; annual-report section map + MD&A commentary trail approved for build (11 Oct) | Tested: chart/table numbers on cited MD&A pages are restated in prose, so image reading adds cost and breaks word-for-word verification for little gain. Section map gives automatic assurance labels and shows MD&A language moving from figures (FY23–FY24) to a single qualitative phrase (FY25–FY26) |
| 34 | Auditor's report check added to Phase 3.4 (11 Oct): opinion type by phrase rules, key audit matter titles, deal-named flag per year; never 'auditor confirms delivery' | Verified: opinions unmodified FY23–FY26; Citi acquisition was KAM #1 in FY23 only (incl. reliance on Citi IT systems till migration). Only independent third-party voice in the sources; dropping after FY23 is expected (one-time acquisition accounting), shown as fact not red flag |
| 35 | Dropped `temperature: 0` from the finder API call (11 Oct) | Live test: Anthropic rejects `temperature` for claude-haiku-5-5 ("deprecated for this model"), so every call failed. Repeatability now rests on code: word-for-word quote check, number check, ledger and fixed status rules. The model's wording may vary run to run; the verified result does not |
| 36 | Finder frozen after Phase 3.7 (11 Oct). Final live scores, Haiku 5.5: dev set 7/10, 8/10, 7/10 across three runs (tuned on these); held-out set (frozen before running) positives 4/6, negatives 3/3 | No invented quote or number passed the word-for-word check in any run; the check rejected the fy23 p.109 paraphrase in all runs and one wrong-page attribution. But 4 verified passages were real yet off-question (e.g. ₹1,106 cr returned for a synergy question; an analyst's question quoted as a 'report'). So 'verified' means only 'these words are on that page'; a person must confirm meaning. No further finder tuning before 17 Oct |
