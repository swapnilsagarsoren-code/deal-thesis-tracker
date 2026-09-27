# Stress tests — "why not just use X?"

These were argued out honestly during planning. Weak defenses were dropped.

## "The user will just use Perplexity"

**Conceded:** For a one-off question ("did the Zomato–Blinkit synergies happen?"), Perplexity gives a decent answer. The live-search feature alone does not beat it.

**What survives:**
- Thresholds set *before* the outcome — Perplexity judges with hindsight every time
- A persistent ledger: same deal, same rules, checked again next year
- Verdict history — chat tools keep no state across sessions
- Every number pinned to a filing quote, checkable in seconds

## "Isn't this just Claude for Financial Services / NotebookLM?"

**Conceded:** A skilled user can upload filings to NotebookLM or Claude and get extraction with citations.

**What survives:**
- They must know *which* line items to check — the Forensic Extraction list encodes an auditor's judgment
- They must set their own thresholds and keep them honest
- They must repeat it for every deal and every year; the tracker does it once, publicly
- Named human review vs anonymous one-off chat output

## "How is this different from a user using Claude?"

The LLM is a component, not the product. The product is the **discipline**: fixed template, pre-registered thresholds, deterministic verdicts, audit-grounded line items, and a public record over time.

## AI-centrality test ("remove the AI — does it fall apart?")

Yes, for extraction: without the LLM, someone must read every filing by hand, which is the bottleneck that stops this from existing today. Contrast: a reference site (India Venture Index) lists an AI model as a data source but its calculation is fully deterministic — the AI looks decorative. Avoid that.
