# RAG Case Study — "Notebook to live endpoint" (draft)

> Proof anchor for the portfolio claim: **I ship ML systems end-to-end, not just
> notebooks.** This is the only full case study. Read in under 2 minutes.

---

## The one-sentence story

I built and deployed a RAG system under a hard GPU-credit budget on Lightning AI,
hit a p95 cold-start spike that made the first request unacceptably slow, and
fixed it with a warm-up health check and retry-backoff in the FastAPI layer —
then published the before/after latency numbers instead of just claiming it works.

---

## System diagram

```
User
  │  HTTPS (POST /query)
  ▼
FastAPI (Retry + backoff on cold start)      ← error handling here
  │
  ├─ Warm-up health check  (GET /healthz → pre-loads model + index)
  │
  ├─ Embedding step  (retrieval: vector index, top-k)
  │
  ├─ Generation step  (LLM over retrieved context)
  │
  ▼
Lightning AI GPU server  (budget-capped, cold-starts allowed)
  │
  ▼
JSON response  (answer + sources + latency stats)
```

Pipeline: **FastAPI → Lightning AI GPU server → live endpoint**. The model and
index are pre-loaded on warm-up so the first real request doesn't pay a cold
start.

---

## The before/after numbers (p50 / p95)

> **FILL IN FROM YOUR DEPLOYMENT LOGS.** The format below is what to capture —
> the table is only real once these are your measured values.

| Metric | Before (no warm-up) | After (warm-up + retry-backoff) | Change |
|---|---|---|---|
| First-request latency (cold instance) | `[p50]` / `[p95]` | `[p50]` / `[p95]` | `[e.g. p95 −78%]` |
| Steady-state latency | `[p50]` / `[p95]` | `[p50]` / `[p95]` | `[…]` |
| Timeout / 401 rate on cold start | `[x%]` | `[y%]` | `[… vs …]` |

> Rules I'm keeping: real logs only, no invented numbers; percentiles not just
> averages (the tail is what users feel); state the GPU-credit cost that forced
> the design.

---

## The failure story (what broke → what I fixed → what it cost)

**What broke.** First requests against a cold Lightning instance were slow
enough to time out / 401 before the model was ready. Uptime looked fine in the
dashboard; the *first real user* paid for it.

**What I changed.**
- Added a `/healthz` warm-up path that pre-loads the model and index (no idle
  GPU credits spent on every single request).
- Added retry-with-exponential-backoff in the FastAPI layer for the transient
  cold-start window.
- Logged latency percentiles (p50/p95) so I could see the tail, not the mean.

**What it cost / what I learned.** The fix traded a small warm-up credit cost
for a ~78% cut in p95 first-request latency (substitute your real number). The
lesson: deployment discipline is measured at the tail and under a budget, not on
a warm benchmark.

---

## Honest limits — "what this isn't"

- Designed for `[N]` concurrent users; above that, retries do the work, not
  scale. (Fill in your real ceiling.)
- Retrieval quality depends on the index; I measured latency, not answer
  quality, in this write-up.
- GPU credits are capped — long idle time means a cold restart, by design.

---

## Book 15 minutes to watch it ship live

See the notebook → endpoint walk in front of you: [**Book 15 minutes** →]

*(Calendar link + email fallback go here. This CTA sits directly under the
failure story — the moment of maximum belief.)*

---

## Other work (one line each)

- sid.ai — deployed chatbot product
- AI Email Assistant SaaS — FastAPI → Railway
- Object Detection & Segmentation — GPU server, Lightning AI
- Internships — FlyRank AI (search intelligence), FDAI

---

## Status

- [ ] Numbers filled in from real deployment logs
- [ ] Diagram matches the live system
- [ ] Calendar CTA linked
- [ ] Readable in under 2 minutes (time it)
