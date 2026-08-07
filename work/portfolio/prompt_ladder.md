# Prompt Ladder — AI Fluency
Weak baseline → final reusable prompt, one layer per version.
Subject: the FlyRank content-performance data (`content_refresh_anonymized.csv`) — real data from my own ML track.
Target audience for every run: a client success manager at an SEO agency who manages 30+ accounts and needs to know where to act this month.

Each version was run on the same model; the output shown is verbatim for that exact prompt.

---

## V0 — Baseline (the kind of prompt I'd have typed a month ago)

**Prompt:**
> Explain this data.

**Output:**
> Sure! This is a dataset about content and SEO performance. It has 30,000 rows and 44 columns with useful metrics like CTR, average position, impressions, and engagement. You can use it to analyze how your content is performing and identify areas for improvement. For example, you might look at which pages have high impressions but low clicks, or which content types perform best. Some columns have missing data, so you may want to clean those first. Overall, this looks like a solid dataset for tracking content performance and making data-driven decisions to grow organic traffic.

**Notes:**
- *What changed:* nothing — this is the baseline.
- *What actually improved:* it didn't. Nothing in here I didn't already know, and nothing I can act on.
- *What still failed:* zero numbers, zero findings, zero reader. It even implies things ("grow organic traffic") that may not be the goal.
- *What I'd try next:* tell the model who's reading it. The biggest weakness is that it has no audience, so it explained a dataset to nobody.

---

## V1 — Layer: defined audience

**Prompt:**
> Explain this data. My audience is a client success manager at an SEO agency who manages 30+ client accounts and needs to know where to act this month. Explain it to them.

**Output:**
> This dataset tracks content performance for your client accounts. To help you act, focus on the engagement columns: CTR, impressions, average position, and engagement rate. Pages that are ranking well (low average position) but getting low CTR may need better titles or meta descriptions. Look for content that lost impressions over the last 30 days compared to the previous 30 days — those pages may be slipping and are worth a refresh. You should also check engagement rate and scroll rate to see if visitors are actually reading the content. Use these signals to prioritize which pages to update first.

**Notes:**
- *What changed:* added a defined audience (the CSM).
- *What actually improved:* it stopped explaining what a "dataset" is and stopped giving data-cleaning advice. It now talks about the reader's actual job — prioritizing which pages to update — and names the columns a CSM would care about.
- *What still failed:* no real findings. "May need better titles" is strategy-shaped advice that could be said about any data; the model is assuming the data looks the way it imagines.
- *What I'd try next:* give it the real structure of this file so it stops assuming columns exist and misreading values.

---

## V2 — Layer: real context

**Prompt:**
> Explain this data. My audience is a client success manager at an SEO agency who manages 30+ client accounts and needs to know where to act this month.
> Context: the file is `content_refresh_anonymized.csv` — 30,000 rows, one row per content page, 32 clients. Columns are trailing-90-day metrics (`impressions_90d`, `clicks_90d`, `ctr`, `avg_position`, `engagement_rate`, `ai_traffic_pct`, `scroll_rate`). Notes: `ctr` is stored ×100 (0.76 = 0.76%); `avg_position == 0` means "no position data", not rank one; some columns are heavily missing. Do not invent columns.

**Output:**
> This file covers 30,000 content pages across 32 clients. Each row is one page, and the metrics are trailing-90-day totals (impressions, clicks, sessions) plus rates (CTR, engagement, scroll, AI-traffic share). Beware two quirks: CTR is ×100, and avg_position of 0 means "no position data" — about 1,205 rows, so exclude them before ranking anything. A large share of rows are missing provider_used (~72%) and word_count (~26%), so don't build conclusions on those columns. Since each row is a page, you can rank pages within the same position band rather than comparing a page-1 page to a deep page.

**Notes:**
- *What changed:* added real context about the file (structure, quirks, missingness).
- *What actually improved:* it stopped inventing and misreading. It now correctly flags CTR ×100, the 1,205 zero-position rows, and the 72%/26% missingness — the exact things that would have produced wrong answers from V1.
- *What still failed:* delivery is one dense paragraph. A CSM skimming it can't find the three things to do; the useful claims are buried.
- *What I'd try next:* force a format with explicit sections so the output is scannable.

---

## V3 — Layer: specified output format

**Prompt:**
> Explain this data. My audience is a client success manager at an SEO agency who manages 30+ client accounts and needs to know where to act this month.
> Context: the file is `content_refresh_anonymized.csv` — 30,000 rows, one row per content page, 32 clients. Columns are trailing-90-day metrics (`impressions_90d`, `clicks_90d`, `ctr`, `avg_position`, `engagement_rate`, `ai_traffic_pct`, `scroll_rate`). Notes: `ctr` is stored ×100 (0.76 = 0.76%); `avg_position == 0` means "no position data", not rank one; some columns are heavily missing. Do not invent columns.
> Output format: return a markdown brief with exactly these sections — "What's in this data" (2 sentences), "Top 3 risks", "Where to act this week", "What I can't tell you". No other sections.

**Output:**
> **What's in this data** — 30,000 content pages across 32 clients, with trailing-90-day performance (impressions, clicks, sessions, engagement) and position/CTR rates.
>
> **Top 3 risks** — 1) CTR is stored ×100 and avg_position 0 means "no data" (1,205 rows), so naive analysis will mis-rank pages. 2) provider_used is missing 72% — don't compare providers. 3) Comparing pages across position bands (a page-1 vs a deep page) will always favor the higher band.
>
> **Where to act this week** — pull pages where impressions are high but clicks are low, and check whether position or title is the cause. Refresh content in the top-3 positions that has slipped over the last 30 days.
>
> **What I can't tell you** — I don't know which clients are worse than their own baseline, and I haven't looked at per-client medians.

**Notes:**
- *What changed:* added an explicit output format.
- *What actually improved:* it's scannable now, and "Top 3 risks" is genuinely useful — it would survive contact with a real CSM.
- *What still failed:* "Where to act this week" has zero numbers — "high but low" is not quantified, so it still isn't actionable. The best section is "What I can't tell you", which is a tell.
- *What I'd try next:* show the model one concrete example of a number-backed insight, so it stops hedging.

---

## V4 — Layer: examples of what good looks like (the honest "this made it worse" moment)

**Prompt:**
> Explain this data. My audience is a client success manager at an SEO agency who manages 30+ client accounts and needs to know where to act this month.
> Context: the file is `content_refresh_anonymized.csv` — 30,000 rows, one row per content page, 32 clients. Columns are trailing-90-day metrics (`impressions_90d`, `clicks_90d`, `ctr`, `avg_position`, `engagement_rate`, `ai_traffic_pct`, `scroll_rate`). Notes: `ctr` is stored ×100 (0.76 = 0.76%); `avg_position == 0` means "no position data", not rank one; some columns are heavily missing. Do not invent columns.
> Output format: return a markdown brief with exactly these sections — "What's in this data" (2 sentences), "Top 3 risks", "Where to act this week", "What I can't tell you". No other sections.
> Example of a good insight: "client_19581e27de holds 7,008 of 30,000 pages (23%) — aggregate numbers are dominated by one client, so judge each client against its own median CTR, not the global average." Match that specificity; every insight you write must contain a number.

**Output:**
> This brief uses client-level specificity, matching the example.
>
> **What's in this data** — 30,000 content pages across 32 clients, trailing-90-day metrics. One row = one page. CTR is ×100; avg_position 0 = no data (1,205 rows).
>
> **Top 3 risks** — 1) client_19581e27de is 7,008 rows (23%), so any global average is really "client_19581e27de + everyone else"; use per-client medians. 2) provider_used missing 72%, so provider comparisons are not possible — matching the example's missingness concern. 3) keyword articles are 27,207 rows (91%), so "content_type" comparisons are really article-vs-article noise; the 697 comparison articles are a different animal.
>
> **Where to act this week** — Pages in page_1 whose CTR sits below their tier median. Tier medians: page_1 0.23, page_3_5 0.06, deep 0.00. Take each client's page-1 pages under half their tier median with 250+ impressions — that's a concrete, defensible list.
>
> **What I can't tell you** — I can't tell you which clients improved month-over-month (no delta column). I can't tell you why provider_used is missing 72% — matching the example, I flag it rather than guess.

**Notes:**
- *What changed:* added one worked example of a number-backed insight.
- *What actually improved:* the numbers finally arrived — 23%, 91%, the tier medians — and "Where to act this week" is now concrete and defensible instead of hedged.
- *What still failed / made it worse:* adding the example made the output *worse* in two ways. It doubled in length, and it started mechanically echoing the example's phrasing ("matching the example", "— matching the example's missingness concern") as filler. It also repeats the same caveat in nearly every section. This is the honest "one ingredient hurt" moment — the example changed *what* it says but also how much.
- *What I'd try next:* add quality criteria — no filler, every claim carries a number or says "no data", state limitations once — and a verification rule so it doesn't pad.

---

## V5 — Layer: quality criteria + verification requirements

**Prompt:**
> Explain this data. My audience is a client success manager at an SEO agency who manages 30+ client accounts and needs to know where to act this month.
> Context: the file is `content_refresh_anonymized.csv` — 30,000 rows, one row per content page, 32 clients. Columns are trailing-90-day metrics (`impressions_90d`, `clicks_90d`, `ctr`, `avg_position`, `engagement_rate`, `ai_traffic_pct`, `scroll_rate`). Notes: `ctr` is stored ×100 (0.76 = 0.76%); `avg_position == 0` means "no position data", not rank one; some columns are heavily missing. Do not invent columns.
> Output format: return a markdown brief with exactly these sections — "What's in this data" (2 sentences), "Top 3 risks", "Where to act this week", "What I can't tell you". No other sections.
> Example of a good insight: "client_19581e27de holds 7,008 of 30,000 pages (23%) — aggregate numbers are dominated by one client, so judge each client against its own median CTR, not the global average." Match that specificity; every insight you write must contain a number.
> Quality criteria: no filler; every claim carries a number or explicitly says "no data"; state limitations once, not per section; a stranger with only this file and no prior knowledge must be able to act on the brief.
> Verification: before writing a claim, check the column exists in the file; if you cannot verify it, say so instead of guessing.

**Output:**
> **What's in this data** — 30,000 content pages across 32 clients; one row per page; trailing-90-day metrics. Two traps: CTR is ×100, and avg_position = 0 means no position data (1,205 rows), not rank one.
>
> **Top 3 risks**
> 1. One client (client_19581e27de) is 7,008 rows (23%) — global averages describe them, not your other 31 accounts. Judge each client against its own median.
> 2. provider_used is missing in 72% of rows and word_count in 26% — do not compare providers or word counts.
> 3. Comparing pages across position bands misleads: page-1 median CTR is 0.23 vs 0.00 for deep pages.
>
> **Where to act this week** — For each client: pages with avg_position > 0, impressions_90d ≥ 250, and CTR below half the client's position-tier median. In the lane slice (22,006 visible pages) that's ~5,564 pages (~25%) — start with the page_1 subset, where the gap between current and tier-median CTR is largest.
>
> **What I can't tell you** — No month-over-month delta columns exist, so "improving vs slipping" is not in this file. trend_direction / trend_pct are label-derived from the window, not independent signals.

**Notes:**
- *What changed:* added quality criteria and a verify-before-claim rule.
- *What actually improved:* it's tight — every claim carries a number, the limitations are stated once, and the action is executable by a stranger (a CSM could hand this to an analyst and get the same list). This is the version I'd send.
- *What still failed:* the "act this week" item is still a *rule the CSM must run*, not a pre-built list. A follow-up that demands the actual top-20 pages (not the rule that finds them) would be the next step.
- *What I'd try next:* ask for the concrete output — "return the top-20 page list for this rule, sorted by gap" — and verify it against the file.

---

## Final reusable prompt (stranger-proof)

Use this when you get an unfamiliar tabular file and need to turn it into something a non-analyst can act on. Fill in the bracket fields from the file's README or `df.describe()`.

> I'm handing you a tabular file and I want a one-page brief a busy manager can act on.
>
> **Audience:** a client success manager at an SEO agency who manages 30+ accounts and needs to know where to act this month.
>
> **Context:** `<filename>` — `<n>` rows, one row per `<unit>`, `<k>` `<groups>`. Key columns: `<list>`. Traps already found: `<e.g. units are ×100; 0 means no data in column X; columns Y and Z are >60% missing>`. Do not invent columns.
>
> **Output format:** a markdown brief with exactly these sections — "What's in this data" (2 sentences), "Top 3 risks", "Where to act this week", "What I can't tell you". No other sections.
>
> **What good looks like:** every insight carries a number and a unit, e.g. "one client holds 23% of rows, so judge each client against its own median, not the global average." Match that specificity.
>
> **Quality criteria:** no filler; every claim carries a number or explicitly says "no data"; state limitations once, not per section; a stranger with only this file and no prior knowledge must be able to act on the brief.
>
> **Verification:** before writing a claim, check the column exists in the file; if you can't verify it, say so instead of guessing.

---

## Self-check

- [x] Six runs total (V0 baseline + V1–V5), each version tied to exactly one named layer
- [x] Notes describe changes in the output, not just the prompt (see V1: "stopped explaining what a dataset is"; V3: "best section is 'what I can't tell you'")
- [x] At least one honest "this made it worse" moment — V4: example added numbers but doubled length and produced mechanical filler
- [x] Final prompt works for a stranger (standalone block above, no session knowledge required)
