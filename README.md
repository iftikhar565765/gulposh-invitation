# Gulposh — a rose-garden Indian wedding invitation

*Gulposh* — "wearing flowers" — is an Indian wedding invitation
reimagined as **a Mughal pleasure garden at golden hour**. The guest
enters through a gulab-silk envelope sealed in a rani wax seal — its
flap lined with woven gold paisley — and walks the garden: the names
flourishing on a sandalwood card beneath a silk shamiyana canopy with
hanging latkans, kalamkari rose vines spilling from the corners,
mogra swags, ghungroo clusters, samai lamps and banana-leaf fans at
its feet, a kamal-talai lotus pond glowing behind it, the story set as
a garden album, the date proclaimed on a shagun band of gulnar night,
the celebrations strung along a gold vine of jaali-star niches, the
palette presented as silk swatches, the festivities hanging from a
diya garland, the purna-kalash courted by parda curtains and banana
stems, a turning kolam countdown, a fountain-pen reply desk and a
marigold-swing farewell. Alternating gulnar nights and sandalwood
papers, joined by running ambi-paisley bands; set in Suranna, Eczar,
Amita, Kalam, Halant and Sarala. No site chrome — it is the garden
itself. Fully customisable in the browser with a guided, auto-saving
visual editor — then published live with one click on Cloudflare.

**Live template pieces**

| Piece | Where |
| --- | --- |
| Guest invitation (envelope opening + ten scenes + RSVP) | `index.html` |
| Visual editor ("Invitation Atelier") | `customize.html` |
| Guest submissions page | `replies.html` |
| Static assets + D1 API | `worker.js`, `functions/` |
| Sample content & copy | `js/config.js` |

## The design system

- **Palette** — sandalwood ivory `#FBF3E2`, gulnar pomegranate night
  `#43111F`, rani gulab `#A4284C`, tamra sun-gold `#D69A45`, gulabi
  blossom `#D8748D`, banana-leaf green `#5A7A3A`, gulab silk `#F0C7B7`
- **Type** — Suranna (grand titles), Eczar (numerals & display),
  Amita (names & script), Kalam (marginalia), Halant (prose),
  Sarala (labels), Tiro Devanagari Hindi (shlokas)
- **Decoration grammar** — every ornament is a hand-drawn inline SVG
  with explicit class-based fills/strokes wired to theme tokens:
  shamiyana canopy + latkans, kalamkari rose vines, ambi paisley,
  bandhani butis, kamal talai medallion, genda-phool strings, purna
  kalash, samai & kuthuvilakku lamps, mor peacock, banana fans,
  mogra jasmine, ghungroo bells, jharokha-dome ring, kolam weave,
  gota sparkles, mehndi-style niche stars
- **Moods** — rani (default), marigold (haldi), peacock (mehndi),
  sindoor — one recolour sweeps the whole garden

## Stack

- **Cloudflare Workers** — static assets (`dist/`) + a small JSON API
- **Cloudflare D1** — stores published invitations and guest replies
  (this repo shares the `kalyanam-invitations` database of the
  kalyanam-invitation template; invitations and RSVPs are keyed per
  invitation id, so the sites never collide)
- **No build framework** — vanilla HTML/CSS/JS; `scripts/build.mjs`
  copies source into `dist/` and cache-busts asset URLs with content
  hashes, and stamps every page with the build version
- GSAP (CDN) for the envelope choreography — everything degrades
  gracefully without it
- Every ornament is a pure inline SVG with explicit fills — no
  `<use>`, no shadow-DOM surprises

## Quick start (local)

```bash
npm install
npx wrangler d1 migrations apply kalyanam-invitations --local
npm run build
npx wrangler dev            # http://localhost:8788
```

The invitation runs in honest demonstration mode until published:
RSVPs validate and preview but are not stored. Deploy with a D1
database (see `DEPLOY.md`) and everything — the atelier, publishing,
guest replies, the submissions page — goes live.

## Deploying to Cloudflare

`DEPLOY.md` walks through it step by step (dashboard path and CLI
path). `wrangler.jsonc` already points at a live D1 database, so the
shortest path is simply:

```bash
npm run build && npx wrangler deploy
```

which publishes the site as its own worker at
`https://gulposh-invitation.<your-subdomain>.workers.dev`.
