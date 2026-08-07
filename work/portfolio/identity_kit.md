# Identity Kit — Siddiq

Applied to the portfolio site (`work/portfolio/site/`) and the Claude Project so every
page and every case study inherits the same look.

---

## Fonts (two, both free via Google Fonts)

| Role | Font | Notes |
|---|---|---|
| Headings | **Space Grotesk** (500–700) | technical, quiet character — says "engineer" without shouting |
| Body | **Inter** (400–600) | plain, readable, near-invisible |

Link used:
`https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Space+Grotesk:wght@500;600;700&display=swap`

## Palette (four colors, calm)

| Role | Hex | Used for |
|---|---|---|
| Paper (background) | `#F7F6F2` | the near-white stage; work is the loudest thing on it |
| Ink (text) | `#1A1917` | body and headings, near-black |
| Main | `#1E4E79` | headings' links, the primary button, the favicon |
| Accent | `#B0602E` | used ONCE per page: the one cold-start number in the hero |

That is the whole palette. No gradients, no extra colors.

## Logo / favicon

Simple `SM` monogram in Space Grotesk on the main blue, rounded square, near-white letters.
`site/favicon.svg`, linked on every page and shown in the site header.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64">
  <rect width="64" height="64" rx="12" fill="#1E4E79"/>
  <text x="32" y="43" font-family="'Space Grotesk', sans-serif" font-size="27" font-weight="700" fill="#F7F6F2" text-anchor="middle">SM</text>
</svg>
```

## Style note (two lines, also standing instruction in the Claude Project)

> Quiet and technical: near-white paper, one deep blue, one warm accent used exactly once
> per page. The work is the loudest thing on the page — the styling only gets out of the way.

---

## Self-check

- [x] Two fonts, named, both free
- [x] Tight palette — 4 hex codes, ~4 colors
- [x] Simple favicon/logo exists (SM monogram, linked on every page + header)
- [x] Two-line style note with a single coherent mood, added to the Claude Project
