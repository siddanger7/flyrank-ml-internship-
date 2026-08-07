# Pressure-test output — saved answer

Run date: Week 1 (AI Fluency). Ran inside the `Internship Portfolio` Claude
Project with the proof statement in context.

## Prompt used (see `pressure_test_prompt.md`)

"Does this sitemap walk one person from landing, to believing me, to booking a
15-minute call — and does it actually prove 'I ship ML systems end-to-end'?
Tell me which pages earn their place, which are wasted, and what is missing.
Be specific, not encouraging."

## Answer (from the pressure-test run)

**Earns its place:**
- **Home hero** — right instinct. One-claim headline + a single CTA. No nav
  menu cluttering it. Good.
- **/work → RAG case study** — this is the only page that actually proves the
  claim. The p50/p95 before/after table and the failure story are the evidence;
  the diagram is the summary. This page IS the portfolio.
- **/contact (calendar)** — correct as the single destination.

**Wasted or weak:**
- **/about** — one paragraph of "who I am" doesn't help a co-founder decide.
  They don't book a call because of your bio; they book because of the cold-start
  story. Fold it into the bottom of the case-study page (two lines) or drop it.
  Right now it's a page whose job the hero already does.
- **/work as a directory page** — if there's only ONE case study, don't give it a
  dedicated directory page. The route should be Home → RAG case study directly.
  A list page with one item looks like a placeholder.

**What's missing:**
- **The number up top.** The hero claims "end-to-end," but the co-founder needs
  the cold-start number (p95 before → after) in the headline zone to keep reading.
  Move the single best number next to the claim.
- **A "what this isn't" line.** One sentence on the case study admitting the
  system's limits (e.g., "only handles N concurrent users; retries on cold
  starts"). A co-founder trusts claims with stated limits. That line is
  currently missing and it's the cheapest credibility purchase on the page.
- **The CTA needs a trigger, not a promise.** "Book 15 minutes to watch a model
  ship live" is good, but put the booking link INSIDE the case study right after
  the failure story — that's the moment of maximum belief.

## What I will change (the one thing)

**Fold `/about` into the bottom of the case-study page and remove it as a
standalone page**, so the site becomes: Home (claim + number) → RAG case study
(diagram + p50/p95 + failure story + booking CTA) → other-work one-liners.
That cuts the sitemap from 4 pages to 3 and removes the one page that does not
move the co-founder toward the call.

Secondary (kept for the build weeks): surface the cold-start number in the hero
and add the honest "what this isn't" limits line to the case study.

## Why this change

Every page must move one person from landing → believing → booking. `/about`
asks them to believe *me as a person*; the case study makes them believe *I can
ship*. Only the latter gets a booking. Removing it makes the sitemap strictly
tighter against the one action.
