# Curated Image Set — Siddiq portfolio

Every image this portfolio needs, matched to the 3-page content map, with the call on
**real capture vs generated** noted for each. The rule: the work is shown with real
captures; only connective tissue (icons, hero texture) is generated, in one locked style.

---

## The map (what the pages actually need)

| Page / slot | What it needs | Real or generated | Call |
|---|---|---|---|
| Home / hero texture | near-invisible line pattern behind the claim | **generated** | connective tissue; must not compete with the one number |
| Home / claim icon | small inline icon beside the claim line | **generated** | simple glyph in kit style |
| Case study / endpoint proof | screenshot of a live request to the deployed RAG endpoint | **real capture** | the proof IS a real server responding — a fake image here would be the whole lie |
| Case study / latency table | p50/p95 before → after | **real capture** | screenshot of the latency logs, not a drawn chart |
| Case study / /healthz | screenshot of the warm-up health check responding | **real capture** | shows the fix, observable |
| Case study / diagram | system diagram (FastAPI → Lightning AI → endpoint) | **real capture** | the diagram is already in the page as code/text; a generated "architecture" image would be stock |
| Contact / you | one real photo | **real photo** | the subject is me; a generated face or avatar is a false claim of identity |

## The generated set (one locked style)

All connective-tissue assets live in `site/assets/`. Locked to the kit: Space Grotesk
geometry, 2px round-cap strokes, main blue `#1E4E79` on paper `#F7F6F2`, near-invisible
opacity. Three icons + one texture:

- `icon-endpoint.svg` — the deployed endpoint (two-node server glyph)
- `icon-healthz.svg` — the heartbeat (the fix)
- `icon-clock.svg` — latency (the tail, not the mean)
- `hero-texture.svg` — a 4-line grid at 12% opacity: present enough to give the page a
  rhythm, quiet enough that the claim stays the loudest thing

## What I rejected, and why (the discernment)

1. **A glowing blue neural network as the hero background.** It's the most overused AI
   image on the internet, and it shouts "AI!" when the portfolio's entire argument is
   *"I ship, and here's the number."* It would compete with the cold-start figure instead
   of getting out of its way. Rejected for the exact reason the kit exists: the work must
   be the loudest thing on the page. Kept the near-invisible line grid instead.

2. **A photoreal "server room / data center" shot for the case study.** It would read as
   stock even if it weren't, and a rack of generic servers proves nothing about *my*
   deployment. The real latency screenshot proves the exact claim the page makes. Generated
   imagery can't carry the proof, so the slot is a real capture — no generation attempted
   for it at all.

3. **A "team handshake / success" image.** Never belongs on a proof page whose only job is
   to get a co-founder to book 15 minutes. The CTA is a calendar booking, not a vibe; an
   aspirational people photo would dilute the one action. Dropped from the map entirely.

4. **A photoreal portrait or AI avatar for Contact.** A generated face presented as me is a
   false claim of identity on a page that asks someone to trust me. Only a real photo of me
   goes there; until I take one, the slot stays empty rather than faked.

## Where real beats generated (the rule, in one line)

Anywhere the page claims something true about my work, a real capture is the evidence and
generation would be the lie; generated images are allowed only where they add texture and
say nothing.

---

## Pending real captures (my hands, not AI)

- [ ] Screenshot: one live `POST /query` to the deployed RAG endpoint (response + latency)
- [ ] Screenshot: `/healthz` warm-up output (the fix in action)
- [ ] Screenshot: latency log lines with p50/p95 before → after
- [ ] One real photo of me for Contact
