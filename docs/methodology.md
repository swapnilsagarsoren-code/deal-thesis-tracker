# Methodology

How this tracker checks Axis Bank's promises about its acquisition of Citi India's consumer business, and what it can and cannot tell you.

## What this is, and what it is not

- **It is** a public-disclosure reconciliation: it puts what Axis promised at deal time next to what Axis later reported, with the exact words and page for each.
- **It is** the kind of check a CFO's office runs on its own deal: did we deliver what we told the market?
- **It is not** proof of fraud, failure or intent. Every source here except one RBI data table is prepared by Axis's management, including the audited accounts.
- **It is not** an investment tool, and it does not track anything live. It covers one deal, checked by hand.

## 1. How promises are picked

- **Only Axis's own public words count.** Sources: the deal presentation and conference call of 1 March 2023, later earnings calls, and the annual reports for FY23 to FY26. Media reports and analyst estimates are never used as targets.
- **Every promise is recorded word for word**, with the document, date and page. If the printed page differs from the PDF page, both are shown.
- **Four promises are tracked:**
  - Complete the integration within 18 months of closing (call, p.5).
  - Save 30–40% of Citi's operating costs within 2 years of closing (deal presentation, p.16).
  - Integration cost of about ₹2,000 crore over 18 months (call, p.8).
  - Earnings per share (EPS) and return on equity (ROE) accretive in calendar year 2024 (deal presentation, p.5).
- **Statements with no number or date are listed, not scored.** Revenue synergies, customer and deposit retention, and promises to share progress are shown as "No target committed". The gap is a finding in itself.
- **Closing-day figures are not promises.** The combined card base, the card market share and the CASA ratio in the deal presentation describe the position on the day of the deal. They are listed separately so they are not mistaken for forecasts.

## 2. How targets and deadlines are set

- **A person reads the source and copies the target.** The code then checks that the target words, number and unit appear exactly inside the quoted promise. If they don't, the status is "Unverifiable".
- **No target is invented.** Where management gave a range, the tracker uses only the range it gave. Where it gave no number, nothing is filled in.
- **Two judgment calls, stated openly:**
  - For the 30–40% cost saving, 30% or more counts as delivered. That lower bound is this tracker's choice, not Axis's.
  - The ₹2,000 crore integration cost was given as an estimate, not a limit. The tracker does not treat it as a ceiling to pass or fail.
- **Deadlines come from the promise's own wording** ("within 18 months", "over 2 years post closing", "in CY24") and the closing date of 1 March 2023. Dates are calendar days in India; a deadline stays open until the end of that day.

## 3. How the status is decided

A fixed set of rules in code decides every status. The same evidence always gives the same answer.

| Status | When it applies |
|---|---|
| Delivered | Confirmed, cited evidence meets the stated target on or before the deadline. |
| Partial | After the deadline, the result is between management's own quoted starting point and the target. For the cost saving: above 0% but below 30%. |
| Missed | After the deadline, complete evidence fails the target. For the cost saving: 0% or less. A milestone is either achieved or not. |
| Too early | The target and deadline are valid, but the deadline has not passed and delivery is not yet shown. |
| Unverifiable | A cited promise, target, deadline or comparable result is missing or unconfirmed. |
| No target committed | A person has confirmed that the promise contains no measurable number or checkable milestone. |

- **Only a result for the deadline can show failure.** An earlier result can show success; a later measurement proves nothing about the deadline.
- **Missing evidence is never read as failure.** If Axis did not report a number, the status is "Unverifiable", not "Missed".

## 4. Why the AI never decides the status

- **How AI is used.** An AI evidence finder has been built and tested on a separate test site. Given a promise, it suggests passages from Axis's own documents. Code rejects any quote that is not word for word on its page.
- **"Verified" means only that the words are on that page**, not that they answer the question. On questions frozen before testing, it found 4 of 6 and correctly found nothing for 3 of 3.
- **It is switched off on this site.** Every finding shown here was entered and checked by a person, and fixed rules, never AI, decide each status.
- **The disclosure timeline is not shown on this site.** It labels which document last repeats a target. In testing, the AI did not return one key sentence, which would have overstated the gap, so the finding shown is from a hand text search.
- **When the finder is used, three checks stand between it and the result:** code confirms the quote appears word for word on that page, a person confirms what it means, and only then do the fixed rules give a status.
- **When the AI finds nothing, it must show what it searched**, so a gap is visible rather than hidden.
- The reason is simple: reading documents is where AI helps; deciding pass or fail must be repeatable and checkable, so it stays in code.

## 5. The extra checks

- **Reporting basis.** When a later claim is measured differently from the original promise, the tracker compares six things: metric, size, baseline, business scope, period and what it is measured against. Each is labelled "Comparable", "Different basis disclosed" or "Insufficient detail". It never concludes that a promise was abandoned or that anyone intended to mislead.
- **Integration-cost reconciliation.** Disclosed amounts are added only when they use the same label, the same units and back-to-back periods. Stamp duty, policy changes, goodwill and the purchase price are listed separately and never added in. Where periods do not match the original estimate, no variance is calculated.
- **Assurance level on every number.** Each figure is labelled as an audited note, annual-report management discussion, or call commentary, so you can see how much checking stands behind it.
- **CFO close-out check.** Six items a CFO's office would need to close out the deal, set against what Axis disclosed. Each is labelled Disclosed, Partly disclosed, Different basis disclosed, Not disclosed or Declined when asked, and each line is marked Observed or Inferred. Items are ranked by how many analysts asked about them on the five calls reviewed; that shows what was asked, not how important it is.
- **Regulator data check.** RBI's monthly bank-wise card statistics are compared with Axis's card figure at closing. RBI counts all outstanding cards and Axis counted cards of consenting customers, so the result is shown as not comparable rather than forced to match.

## 6. The honest sequence

- This review is **retrospective**. The deal was chosen and its later results were read in the same period. The thresholds were not fixed before the outcome was known, and the tracker does not claim they were.
- What it does do: record each promise exactly as stated, set the rules before applying them, and show every source, so anyone can check the work and reach the same status.

## 7. Limits

- All sources except one RBI data table are prepared by Axis's management. Even audited accounts reflect management's choices.
- Not disclosing a number is not the same as not delivering it. A missing disclosure is shown as a gap, never as a failure.
- Sources were searched by text. A figure in a table with different wording could be missed.
- This covers one deal, in depth. It is a working prototype, not a complete record of Indian M&A.

## Sources reviewed

Deal presentation and conference call (1 March 2023); completion announcement; earnings calls for Q4 FY24, Q1 FY25, Q2 FY25 and Q4 FY25; Axis Bank annual reports FY2022-23, FY2023-24, FY2024-25 and FY2025-26; RBI bank-wise ATM/POS/card statistics for January, February and March 2023. All are linked from the tracker with page numbers.
