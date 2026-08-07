# Portfolio site (static draft)

3-page static site matching the sitemap (Home → RAG case study → Contact).

| File | Page |
|---|---|
| `index.html` | Hero: the claim + cold-start number + CTA |
| `rag-case-study.html` | The proof: diagram, p50/p95 table, failure story, limits, booking CTA |
| `contact.html` | The one action: book 15 minutes |
| `style.css` | Shared minimal styles (no framework) |

## Deploy (Netlify, free)
1. Push this folder (or the whole repo) to GitHub.
2. Netlify → "Add new site → Import an existing project" → pick the repo.
3. Build command: none. Publish directory: `work/portfolio/site/`.
4. Site deploys at a `*.netlify.app` URL. Put that URL in `submission/paper_url.txt` at the capstone.

## Before it's "true" (fill-ins)
- `[p50]` / `[p95]` / `[x%]` / `[y%]` in the tables ← your real deployment logs
- `[78]` in the failure story ← your real improvement number
- `[N]` concurrent-users ceiling ← your real test number
- Calendly link + real email in `contact.html`
- Replace `[p95-before]%` / `[p95-after]%` in the hero with the real figures
