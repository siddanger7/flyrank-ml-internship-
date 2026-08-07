# The Through-Line — one claim, one map, one action

---

## 1. The one-line claim

**AI's ten options (generated, then whittled):**

1. I ship ML systems end-to-end, not just notebooks.
2. Models are easy; keeping them serving is the work.
3. From notebook to a live endpoint, with the numbers to prove it.
4. I'm the person who carries a model from a notebook to a running server.
5. I build and deploy ML systems that stay up.
6. Where most interns stop at the notebook, I ship the endpoint.
7. The model is the easy part; I do the part after.
8. Deployed, measured, and honest about its limits.
9. I take models to production and write down where they break.
10. Watch a model go from notebook to a live endpoint.

**Chosen and sharpened:**
> **"I ship ML systems end-to-end, not just notebooks."**

Why this one: it states the contrast (notebooks = where most people stop), the action
(ship), and it's *provable* — the RAG deployment is the evidence. #2 and #7 are punchier
but swap the contrast for a complaint; #10 is a CTA, not a claim. This sentence greets the
visitor on Home and every page sends them toward proving it.

---

## 2. Content map (pages → sections → case → CTA)

The ladder: **Home states the claim → the case study proves it → Contact books the call.**
Every CTA points at the one action from Week 1: **book 15 minutes to watch a model go
notebook → live endpoint.**

### Home
1. Hero: the one-line claim + the cold-start number (p95 before → after)
2. CTA → **Case study** (not Contact — route to proof first)
3. Audience line: "for technical co-founders with a research scientist but no one to carry models to production"
4. Footer → Contact
- **CTA on page:** "See the case study"

### Case study (the lead — strongest work first)
1. Title + 2-line summary (claim restated)
2. System diagram: FastAPI → Lightning AI GPU server → endpoint
3. **Before/after p50/p95 table** (the numbers, not claims)
4. Failure story: what broke → what I changed → what it cost
5. Honest limits line ("what this isn't": N concurrent users, retries)
6. **Booking CTA in the moment of max belief** (right after the failure story)
7. About, 2 lines (bottom)
8. Other work, one line each (no pages): sid.ai · Email Assistant SaaS · Object Detection · FlyRank + FDAI
- **CTA on page:** "Book 15 minutes to watch it ship live"

### Contact
1. The action restated: "Book 15 minutes… you bring a hard question, I bring a deployed system"
2. Calendar button (the one action)
3. Email fallback
- **CTA on page:** calendar booking + email

Other-work cases deliberately get **no pages** — they would distract from the one claim
(decision recorded in `sitemap.md`; full framings live in `framed_cases.md` if ever needed).

---

## 3. Still need to gather (honest list, so the build week isn't blocked)

**Blocking the proof page:**
- [ ] RAG p50/p95 latency numbers (before → after) from the deployment logs — the one real number the claim leans on
- [ ] Real captures: live `POST /query` response, `/healthz` warm-up output, latency log lines
- [ ] Confirm the deployed endpoint is reachable → live demo link for the page

**Non-blocking:**
- [ ] One real photo of me (Contact)
- [ ] Real calendar URL to replace the placeholder on Contact
- [ ] Testimonial — none yet; optional, only if a co-founder/mentor offers one honestly

**Internship work not finished yet (will land on the page later, one-line each):**
- [ ] ML-05 → ML-11 + capstone: warehouse feature-leakage check, baseline, model, validation audit, action playbook, deployed research paper (`submission/paper_url.txt`)
- [ ] The capstone paper itself is the eventual "second full case" candidate — not yet earned, so it does not compete with RAG for the lead

---

## Self-check

- [x] Single memorable claim, one sentence
- [x] Every page has ordered sections + a named CTA; the strongest work (RAG) leads
- [x] CTAs ladder to the one action from Week 1 (book 15 minutes)
- [x] Gather-list is honest — proof-page blockers listed first, unfinished internship work named
