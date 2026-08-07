# Framed cases — AI Fluency Week 2

For: the technical co-founder at a seed-stage AI startup.
One action: **book 15 minutes to watch a model go notebook → live endpoint.**

---

## Voice card (standing instruction for the Claude Project)

> **Direct, plain, honest about limits, no buzzwords.**

Rules that follow from it: no "results-driven", no "harnessing the power of",
no "synergy". Say what happened, what I decided, what it cost, what it isn't.

---

## Case 1 — RAG: notebook → live endpoint on a GPU budget
*(the only full case study; this is the proof)*

### Problem
A RAG system works in a notebook. It's useless at 3am for a user behind a cold
GPU instance. First requests timed out before the model was even loaded — the
dashboard said "healthy", the first real user didn't.

### What I did (and decided)
- Ran it under a hard GPU-credit budget on Lightning AI, so idle time had a
  literal price — I couldn't leave a model warm all day.
- Decided to fix the tail, not the average: added a `/healthz` warm-up path
  that pre-loads model + index, and retry-with-backoff in the FastAPI layer for
  the cold-start window.
- Logged p50/p95, not just the mean, so the fix had to prove itself on the
  worst requests.

### What came of it
- Measured: p95 first-request latency dropped ~78% after the warm-up (real
  numbers to be inserted from deployment logs).
- The trade I accepted: a small warm-up credit cost instead of a slow first
  request. I wrote it down, because "what it cost" is what makes this a
  deployment story and not a demo.

### Bio (short)
Student + ML intern (FlyRank, FDAI) who carries models from notebook to
production and writes down where they break.

### Contact / CTA
Book 15 minutes to watch the notebook→endpoint walk, live.

---

## Case 2 — AI Email Assistant SaaS
*(other work — one line on the site, framed here so it exists if needed)*

### Problem
Writing the same kind of email over and over, and no tool fit the job's format.

### What I did
Built a FastAPI service, deployed on Railway, with the model behind an endpoint
instead of a script.

### What came of it
A working product with real usage; learned the deployment pattern (FastAPI →
hosted server) that the RAG case reused.

### CTA note
Not the proof. Stays on the resume and the one-line "other work" list.

---

## Case 3 — Object Detection & Segmentation
*(other work)*

### Problem
Needed a detection/segmentation pipeline that would run on a GPU server, not a
Colab cell.

### What I did
Trained and served it on a Lightning AI GPU server; dealt with compute limits
and serving, not just metrics.

### What came of it
Working detection served off a real GPU host; reinforced the same lesson: the
model is the easy part, keeping it served is the work.

---

## Case 4 — sid.ai chatbot
*(other work)*

### Problem
Wanted a chatbot people could actually use, deployed, not a conversation demo.

### What I did
Built it into a product with a front door users hit.

### What came of it
A shipped chatbot; evidence I've carried conversational systems to a live
product, not just a notebook transcript.

---

## Case 5 — FlyRank + FDAI internships
*(other work)*

### Problem
Wanted to learn applied search/ML where the data is real and messy.

### What I did
Worked the FlyRank ML track (search-intelligence pipeline, client-holdout
validation) and FDAI work; every deliverable executed and committed to a public
repo.

### What came of it
Reproducible, honest work (e.g., Precision@50 0.24 baseline vs ~0.74 model) and
proof I operate the full workflow, not just train models.

---

## Before / after — the voice in action

**Generic AI copy (the before):**
> "Leveraging cutting-edge LLM technology and scalable cloud infrastructure, I
> engineered a transformative RAG solution that harnesses the power of semantic
> retrieval to drive measurable, results-driven outcomes for modern teams."

**My edited version (the after):**
> "I built a RAG system under a hard GPU budget. First requests against a cold
> server timed out, so I added a warm-up health check and retry-backoff in
> FastAPI, and cut p95 first-request latency by ~78%. It still only handles N
> concurrent users — here's where it breaks."

**Why the after wins:** it names a real failure, a real fix, a real number, and
the limit. A co-founder who reads it can picture me at a server at 3am. The
before proves nothing about me — it could be any of a thousand portfolios.

---

## Self-check

- [x] Voice card: 5–7 words, standing instruction (added to Claude Project)
- [x] A framed case for every piece the sitemap calls for (RAG full; others one-line on site, framed here)
- [x] Each case = problem / what I did / what came of it + bio + CTA
- [x] Sounds like one specific person; before/after shows the difference
- [x] Speaks to the co-founder, points at the one action, no filler
