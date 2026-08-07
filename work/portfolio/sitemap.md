# Portfolio Sitemap — Week 1 sketch (text version, post pressure-test)

Target reader: a technical co-founder at a seed-stage AI startup.
One action: **book 15 minutes to watch a model go notebook → live endpoint.**

**Pressure-test change applied:** `/about` was folded into the bottom of the
RAG case-study page and removed as a standalone page. Three pages, not four —
every page now moves the co-founder toward the call.

```
/  (Home)
   ├── Hero: "I ship ML systems end-to-end, not just notebooks."
   │        + the cold-start number (p95 before → after) beside the claim
   │        + CTA → RAG case study (not /contact)
   ├── /work/rag-case-study   (THE proof — the only full case study)
   │   ├── System diagram: FastAPI → Lightning AI GPU server → endpoint
   │   ├── p50/p95 cold-start before/after table (the numbers, not claims)
   │   ├── The failure story: what broke, what I fixed, what it cost
   │   ├── Honest limits line: "what this isn't" (N concurrent users, retries)
   │   ├── Booking CTA right after the failure story (moment of max belief)
   │   └── About (2 lines, bottom): who I am, why shipping is my focus
   │   └── Other work (one line each, links only): sid.ai chatbot,
   │       AI Email Assistant SaaS, Object Detection & Segmentation,
   │       FlyRank + FDAI internships
   └── /contact     (the ONE action: calendar booking + email fallback)
```

## Why each page earns its place

| Page | Job it does for the co-founder | Would removing it hurt? |
|---|---|---|
| **Home (hero)** | States the claim in one breath + the cold-start number; routes straight to the proof. | Yes — nothing would state the claim. |
| **RAG case study** | The entire evidence: diagram, p50/p95 table, failure story, limits line, and the booking CTA in the moment of belief. | Yes — this IS the portfolio. |
| **Contact** | The single action: book 15 minutes. | Yes — there'd be nowhere to act. |

## Pages deliberately NOT included

- Standalone `/about` — folded into the case study (a bio doesn't book a call).
- Full case studies for the other projects — they'd distract from the one claim.
- Blog, skills cloud, testimonials, services/pricing — none earn the booking.

## Proof anchors required before this is "true"

- [ ] RAG case-study page exists (diagram + p50/p95 table + failure story + limits line, <2 min read)
- [ ] Cold-start number visible in the hero beside the claim
- [ ] Other projects demoted to a one-line "other work" list
- [ ] Calendar booking CTA sits directly under the case study failure story
