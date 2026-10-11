# Design — how it should look and feel

## Feel

An audit working paper, not a dashboard: quiet, document-like, every claim cited. It should be credible to a CFO in 10 seconds.

## Type

Manrope for headings, DM Sans for body text.

## Colour

- Brand: wine `#861747` (accent and links), ink `#243038`, muted `#65727b`, line `#dce1e5`, page `#f5f6f8`.
- Status: Delivered `#25805b`, Partial `#a57614`, Missed `#b64040`, Too early `#347db3`, Unverifiable / No target `#657580`.
- Status is always a dot plus the word, never colour alone.

## Layout

- Maximum width 1184px.
- White cards with a 1px border and 10–12px radius; each promise card has a left stripe in its status colour.
- Below 768px, tables stack into cards.

## Screen order

1. Hero
2. Promised-vs-reported summary
3. One-line finding
4. Four promise cards: status → threshold and deadline → promise vs result → finder line
5. Reporting-basis comparison
6. Integration-cost reconciliation
7. Auditor's report check, shown once
8. Items considered with no target committed
9. Methodology link

## Rules

- Every number links to a source page.
- Every reported figure carries an assurance label.
- Empty states say why ("not disclosed", "not run for this promise"), never "N/A".
- No charts unless every period has a disclosed number.
- Wording: "Different basis disclosed" and "not disclosed"; never "failed" or "misled".
