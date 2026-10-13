# Product spec — M&A Value Realization Tracker (India)

*Current as of 13 Oct 2026. Live since 13 Oct: https://doting-zebra-426.convex.site.*

*Framing (13 Oct, decision log row 41): can anyone outside tell whether the acquirer delivered what it promised? The main finding is the missing bridge between the number announced and the number later tracked.*

## Problem

After a deal, management promises value. Later reporting often drops the promise, changes its basis, or stops reporting it. Few people line up the promise against what was actually reported.

## Who it is for

The check a CFO's office or integration team would run on its own deal, shown on public data. It is not an investor tool and makes no claims about fraud or intent.

## Scope

- One deal: Axis Bank × Citi India Consumer (closed 1 Mar 2023).
- Sources: 11 Axis documents, namely the deal deck, the closing call, the completion announcement, 4 earnings calls and the FY23–FY26 annual reports.

## Top 3 features

1. A promised-vs-reported summary of 4 promises. The status comes from six fixed rules, never from AI.
2. Cited evidence cards: every quote is linked to its page with an assurance label. They include the integration-cost reconciliation and a reporting-basis comparison.
3. An AI evidence finder (Claude Haiku):
   - It suggests passages, and code checks every quote word for word.
   - Nothing is shown publicly until a person accepts it.
   - Test results: practice set 7–8/10; held-out set 4/6 found, and 3/3 correctly found nothing.
   - A disclosure timeline (which document last repeats a target) is built but hidden on public cards: in live tests the finder missed the key FY23 sentence (decision log row 37). The finder is switched off on the live site.

## What it doesn't do

Other deals; live search; forecasts; calling silence "failure"; claiming audited assurance for management commentary.

## Method

- The review is retrospective, not outcome-blind.
- A promise with no number or date is marked "No target committed".
- A changed basis is marked "Different basis disclosed".

## Success check (17 Oct)

- [ ] A stranger sees all 4 promises and their statuses within 10 seconds.
- [ ] Every number opens its source page.
- [ ] The methodology page states the retrospective sequence.
- [ ] 3 people have used it, and one comment from each is logged.
- [ ] The demo video is recorded.

## Stack

- React + Vite on Convex: dev `deafening-sparrow-421`, prod `doting-zebra-426`.
- The finder sits behind a kill switch with daily limits.
