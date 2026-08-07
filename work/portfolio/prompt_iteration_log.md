# Prompt Iteration Log — AI Fluency
**Task:** FL-01 Target Task C — *Turn a raw doc/notebook into concise working notes*
(<300 words, covers every "gotcha", no invented numbers; I could answer a quiz from it alone).

**Real input used:** `docs/ml-intern-dataset-and-lane-guide.md` (the actual lane guide for my own ML track — 728 lines of real warnings I have to work from this internship).

Each version applies exactly one named technique on top of the previous. Outputs are verbatim model replies to that exact prompt.

---

## V0 — Naive prompt (what I would have typed a month ago)

**Prompt:**
> make me notes from this document

**Output:**
> Here are some notes on the FlyRank dataset guide. It describes a machine learning internship at FlyRank focused on search intelligence. The main dataset is on Hugging Face and needs access approval. There are four lanes you can pick: ranking signal analysis, refresh scoring, clustering, and CTR engagement. The guide explains how to use the data, suggests models like logistic regression and random forests, and warns about things like leakage. The starter data has 30,000 rows. You should build a baseline before a model and validate your results. Overall it's about ranking pages for content decisions.

**Note (what changed / observed / why):**
- *Changed:* nothing — baseline.
- *Observed output difference:* it's a chapter-by-chapter summary I could get from the title alone. Zero of the actual warnings survived: no CTR ×100, no `avg_position == 0`, no freshness cut, no sparse AI sessions. If I studied from this I would mis-rank pages on week one.
- *Why this matters:* the output proves the model read the headings, not the content. My prompt gave it no reason to extract the parts that could actually break my pipeline.

---

## V1 — Technique: role assignment

**Prompt:**
> You are a senior ML engineer at FlyRank mentoring an intern who is one week in. Make working notes from this document that an intern can act on: `docs/ml-intern-dataset-and-lane-guide.md`

**Output:**
> This guide explains the internship's whole workflow: research question → data contract → signal audit → baseline → lane → validation → ranked recommendations. Your main data source is the warehouse release on Hugging Face (gated, approval is instant). There are four core lanes and two freestyle directions. Before you model, sort every column into buckets: join keys, observed signals, derived measurements, product context, targets, raw-origin context. Always build a baseline first, and never feed the product's own decision flags into your model or you get a circular result. Validate the way the decision is used: client-holdout, time-aware splits, top-K review. When in doubt, keep claims observational — you watched, you didn't experiment.

**Note (what changed / observed / why):**
- *Changed:* assigned the model a role (senior engineer mentoring an intern).
- *Observed output difference:* the framing switched from "explain the document" to "tell an intern what to actually do" — the workflow order and the column-bucketing habit came through, which is the kind of practical structure a mentor would emphasize. Tone got more concrete.
- *What still failed:* still no numbers and no specific traps. "Never feed product flags" is right but "CTR is stored ×100" is missing. A role alone changes priorities, not precision.
- *Next:* add why the notes exist (my motivation), so it keeps the warnings that break a real build.

---

## V2 — Technique: context and motivation

**Prompt:**
> You are a senior ML engineer at FlyRank mentoring an intern one week in. These notes are for the intern to build their data contract and run validation from, next week. Every warning that would break the pipeline must be stated concretely. Skip sales language and history. `docs/ml-intern-dataset-and-lane-guide.md`

**Output:**
> Your data contract must name one row's meaning (grain) before you touch any table. The warehouse has dimension tables (one row per client / content) and a fact table (one row per item per day) — do not flatten the daily fact table without choosing a decision grain. Daily facts stop at 2026-06-30: the freshest 3 days were cut on purpose, and clients start their history at different dates (check gsc_data_start / ga4_data_start) or "no tracking yet" will look like "no traffic." Treat the starter label is_declining_label (trend_direction == "down") as a proxy, not a capstone target. Pick validation by how the list is used: client-holdout for clients, time-aware for future movement, top-K for ranked queues, leakage audit always.

**Note (what changed / observed / why):**
- *Changed:* added context (what the notes will be used for) and motivation (break the pipeline).
- *Observed output difference:* the model finally kept the concrete gotchas — the 2026-06-30 freshness cut, the unbalanced panel check, `is_declining_label` as proxy. These are exactly the facts that break a real build if missed. Relevance jumped.
- *What still failed:* it's still prose, and it dropped the single most common unit trap (CTR ×100) and `avg_position == 0`. Precision improved but completeness didn't — nothing forced the model to enumerate traps exhaustively.
- *Next:* show it the standard I want with a few-shot example, so "concrete" has a concrete meaning.

---

## V3 — Technique: few-shot examples

**Prompt:**
> You are a senior ML engineer mentoring an intern. These notes are for building the intern's data contract and validation next week; every pipeline-breaking warning must be concrete. Skip sales language.
> Standard for a note:
> GOOD — "CTR is stored ×100, so 0.76 means 0.76%, not 76%."
> BAD — "Watch out for CTR units."
> GOOD — "trend_direction / trend_pct are label-derived buckets from the current window; never use them as features."
> Capture the document's traps the same way: a specific column or number, then what it means. `docs/ml-intern-dataset-and-lane-guide.md`

**Output:**
> - CTR is stored ×100 (0.76 = 0.76%). avg_position == 0 means "no position data" (1,205 rows in the starter set) — never read it as rank one.
> - trend_direction / trend_pct are buckets derived from the current window — label/proxy material, never features.
> - Product decision flags (health_score, priority_score, action_type) are NOT in your data; rebuild one and it's a baseline to beat, never a feature (circular result).
> - Daily facts stop 2026-06-30; freshest 3 days cut on purpose. Different clients start at different dates — check dim_clients.gsc_data_start and ga4_data_start before defining any window.
> - AI sessions are sparse: 30,177 rows with AI sessions vs 78.8M daily rows. No binary classifier on AI sessions alone.
> - Warehouse daily fact table is 78,835,655 rows; grain is daily × client × content — choose a decision grain before flattening.
> - Validation: client-holdout for clients, time-aware for future movement, top-K for ranked queues, leakage audit every time.
> - Starter label is_declining_label (trend_direction == "down") is a proxy, not the capstone target.

**Note (what changed / observed / why):**
- *Changed:* added two few-shot examples defining the exact specificity standard.
- *Observed output difference:* this is the first version that looks like notes I could study from. Every line is "a column or number → what it means," and traps that V1 and V2 missed (CTR ×100, avg_position == 0, 30,177 vs 78.8M) appeared. The examples taught the *shape* of an answer, not the content.
- *What still failed:* it's a flat list — nothing groups unit traps vs data-availability traps vs validation rules, so it's hard to quiz from, and it's over 300 words.
- *Next:* force an output structure so the coverage is complete and scannable.

---

## V4 — Technique: output structure

**Prompt:**
> You are a senior ML engineer mentoring an intern. These notes are for building the intern's data contract and validation next week; every pipeline-breaking warning must be concrete. Skip sales language.
> Standard for a note: GOOD — "CTR is stored ×100, so 0.76 means 0.76%, not 76%." BAD — "Watch out for CTR units." Capture traps the same way: a specific column or number, then what it means.
> Output structure: under 300 words, exactly four sections — [1] Units and encoding traps, [2] Data availability and panel warnings, [3] Label and leakage rules, [4] Validation that matches the decision. Bullets only. `docs/ml-intern-dataset-and-lane-guide.md`

**Output:**
> **[1] Units and encoding traps**
> - CTR stored ×100 — 0.76 means 0.76%, not 76%.
> - avg_position == 0 means "no position data" (1,205 rows in starter), never rank one.
> - trend_pct / trend_direction are window-derived buckets — not independent signals.
>
> **[2] Data availability / panel**
> - Daily facts stop 2026-06-30 (freshest 3 days cut on purpose).
> - Unbalanced panel: each client's history starts at its own date; check gsc_data_start / ga4_data_start before windows.
> - AI sessions sparse: 30,177 vs 78.8M daily rows — ranking/EDA only, no binary classifier.
> - Grain: 78,835,655 daily × client × content rows; pick a decision grain before flattening.
>
> **[3] Label / leakage rules**
> - Product flags (health_score, priority_score, action_type) not in data; rebuilt = baseline, never a feature (circular result).
> - is_declining_label (trend_direction == "down") is a proxy label, not the capstone target.
> - Feature window must never overlap target window.
>
> **[4] Validation that matches the decision**
> - Client-holdout for clients; time-aware for future movement; top-K for ranked queues; leakage audit every time.
> - No causal claims (refresh → recovery) without an experiment.

**Note (what changed / observed / why):**
- *Changed:* imposed a four-section structure with a word cap.
- *Observed output difference:* complete and scannable — the grouping surfaced coverage gaps (adding section [4] pulled in the causal-claims rule that V3 dropped), and the 300-word cap forced compression without losing the numbers. This is the first version that meets Target Task C's bar.
- *What still failed:* nothing major; the last real risk is that it's produced from memory of the doc's structure, not a methodical sweep — a step-decomposition could guarantee nothing is skipped.
- *Next:* add step decomposition to force exhaustive enumeration before writing.

---

## V5 — Technique: step decomposition

**Prompt:**
> You are a senior ML engineer mentoring an intern. These notes are for building the intern's data contract and validation next week; every pipeline-breaking warning must be concrete. Skip sales language.
> Standard for a note: GOOD — "CTR is stored ×100, so 0.76 means 0.76%, not 76%." BAD — "Watch out for CTR units."
> Work in three steps BEFORE writing: (1) list every unit/encoding warning in the doc; (2) list every data-availability/panel warning; (3) list every label/leakage/validation rule. Then write the notes as bullets under exactly four sections — [1] Units and encoding traps, [2] Data availability and panel warnings, [3] Label and leakage rules, [4] Validation that matches the decision. Under 300 words. `docs/ml-intern-dataset-and-lane-guide.md`

**Output:**
> **[1] Units and encoding traps**
> - CTR stored ×100 — 0.76 means 0.76%, not 76%.
> - avg_position == 0 = "no position data" (1,205 rows in starter), never rank one.
> - trend_direction / trend_pct are current-window buckets — features would leak the label.
> - Rows from before a client's GA4 start hold search data only (ga4_data_available = FALSE) — don't read "no sessions" as "no traffic."
>
> **[2] Data availability / panel**
> - Daily facts stop 2026-06-30 (freshest 3 days cut on purpose).
> - Unbalanced panel — each client starts its history at its own date; check gsc_data_start / ga4_data_start.
> - AI sessions sparse (30,177 vs 78.8M daily rows) — EDA/ranking only, no binary classifier.
> - Grain: 78,835,655 daily × client × content rows; choose a decision grain before flattening.
> - For experiments use a middle month (e.g. 2026-03); avoid the _sample table (it holds the LAST month — future-peeking).
>
> **[3] Label / leakage rules**
> - Product flags (health_score, priority_score, action_type) not in data; rebuilt = baseline to beat, never a feature (circular result).
> - is_declining_label (trend_direction == "down") is a proxy, not the capstone target.
> - Feature window never overlaps target window.
>
> **[4] Validation that matches the decision**
> - Client-holdout for clients; time-aware for future movement; top-K for ranked queues; leakage audit every time.
> - No causal claims (refresh → recovery) without an experiment.
> - Public-safe output: no raw queries/URLs/titles; keep claims observational.

**Note (what changed / observed / why):**
- *Changed:* added step decomposition (enumerate by category first, write second).
- *Observed output difference:* two warnings appeared that V4 missed — the `ga4_data_available = FALSE` trap and the `_sample`-table-future-peeking rule — because the model was forced to sweep category by category instead of recalling the doc's section order. Same techniques, more complete because of the explicit steps.
- *What still failed:* minor — a couple of bullets could be tightened, and the word count is near the cap.
- *Next:* nothing structural; this is the final prompt. The remaining work is the cross-model check.

---

## Final prompt (run on both Claude and ChatGPT)

> You are a senior ML engineer mentoring an intern. Produce working notes from `docs/ml-intern-dataset-and-lane-guide.md` for building the intern's data contract and validation next week; every pipeline-breaking warning must be concrete. Skip sales language and history.
> Standard for a note: GOOD — "CTR is stored ×100, so 0.76 means 0.76%, not 76%." BAD — "Watch out for CTR units." Capture traps the same way: a specific column or number, then what it means.
> Work in three steps BEFORE writing: (1) list every unit/encoding warning; (2) list every data-availability/panel warning; (3) list every label/leakage/validation rule. Then write the notes as bullets under exactly four sections — [1] Units and encoding traps, [2] Data availability and panel warnings, [3] Label and leakage rules, [4] Validation that matches the decision. Under 300 words. No invented numbers.

**Output on Claude** (V5 output above; same model, same result — reproduced cleanly on re-run).

**Output on ChatGPT** (verbatim, condensed to the final sections — it also printed its three-step working first):
> **[1] Units and encoding traps**
> - `client_hash_id`, `content_hash_id`, `url_hash_id`, and `keyword_hash_id` are scrambled IDs; join/group with them only.
> - `ga4_data_available = FALSE` means tracking had not started, not zero sessions.
> - Recreated product scores belong in baselines, not model features.
>
> **[2] Data availability and panel warnings**
> - Daily facts end at **2026-06-30** because the newest 3 days were removed.
> - Client histories start on different dates; check `gsc_data_start`/`ga4_data_start` before windowing.
> - AI-session data is much sparser than search-performance data.
> - Do not use the `_sample` table for future-label development.
>
> **[3] Label and leakage rules**
> - Define labels from future outcomes.
> - Never overlap feature and target windows.
> - Never use future metrics or rebuilt product outputs as features.
>
> **[4] Validation that matches the decision**
> - Use client holdout for client generalization.
> - Use time-aware splits for future prediction.
> - Use top-K metrics for ranked review queues.
> - Run a leakage audit before trusting results.

---

## Cross-model comparison (Claude vs ChatGPT)

| Dimension | Claude | ChatGPT |
|---|---|---|
| Tone | Measured, hedged ("never read it as rank one", "no binary classifier on AI sessions alone") | Assertive, states rules as absolutes ("Define labels from future outcomes") |
| Accuracy on traps (×100, avg_position==0, 1,205 rows) | All three present and correct, with the number | **Both ×100 and avg_position==0 are missing** — the two traps most likely to produce wrong rankings. ChatGPT's "[1] Units and encoding traps" instead leads with scrambled join IDs, which are real but are the least dangerous item in the category |
| Specificity of numbers | Carries them: 30,177 vs 78.8M daily rows; 1,205 rows; 2026-06-30 | Vague where Claude is specific: "AI-session data is much sparser" with no numbers; 2026-06-30 present |
| Label / proxy awareness | Calls out `is_declining_label` as a proxy, not the capstone target | Omits `is_declining_label` entirely; also omits the "no causal claims (refresh → recovery)" rule |
| Structure adherence | Follows the four sections; occasionally runs a few words over 300 on re-runs | Follows the four sections exactly AND prints its step-decomposition first — total output well over 300 words |
| Failure points | Word-cap drift; occasional extra bullet | Missing the encoding canaries; fills space with low-value items; duplicated working output |

**Verdict (specific, not "both fine"):** on this task ChatGPT is the more fluent writer and the worse note-taker. It nailed the structure and the panel warnings, but it dropped the two encoding traps (CTR ×100, avg_position == 0) that would actually produce wrong numbers in the intern's pipeline — and it wrote "sparser" where the numbers (30,177 vs 78.8M) would have made the point. Claude hedged more and carried the numbers. For a task whose whole point is "every pipeline-breaking warning must be concrete," the difference is decisive: Claude's output was the one I'd study from.

---

## Final reusable template (stranger-proof)

Use for turning any dense technical doc into working notes. Fill the brackets.

> You are a senior <role> mentoring a <reader>. Produce working notes from <source doc> for <what the reader will build next>; every <context-specific>-breaking warning must be concrete. Skip <language to exclude>.
> Standard for a note: GOOD — "<concrete example: column/number + what it means>". BAD — "<vague version of the same>". Capture warnings the same way: a specific column or number, then what it means.
> Work in three steps BEFORE writing: (1) list every <unit/encoding warning>; (2) list every <availability/panel warning>; (3) list every <label/leakage/validation rule>. Then write the notes as bullets under exactly four sections: <list sections>. Under <word cap> words. No invented numbers.

---

## Self-check
- [x] Six runs (V0 naive + V1–V5), each tied to a named technique (role, context, few-shot, structure, decomposition)
- [x] Notes describe observed output differences, not just prompt changes (V1 tone shift, V2 relevance jump, V3 shape-learning, V4 coverage gaps, V5 new warnings)
- [x] Real task from FL-01 audit (Target Task C, using the actual lane guide)
- [x] Cross-model comparison completed (Claude vs ChatGPT run on the same final prompt; see table above)
- [x] Final template reusable without my personal context
