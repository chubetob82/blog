# blog.bolivarjesus.com

Static site served by GitHub Pages from the repository root. No build step, no framework —
hand-written HTML, one file per page. `.nojekyll` is present, so files are served exactly as
committed.

## Pages

| Path | What it is |
| --- | --- |
| `index.html` | Landing page: masthead, the latest piece, a short "about", contact. Bilingual EN/ES in a single file. |
| `KangarooPegRevisited2026/` | The Kangaroo Peg, 2026 (English) |
| `KangarooPegRevisited2026/es/` | La paridad canguro, 2026 (Spanish) |
| `404.html` | Not-found page |
| `www-preview/` | **Prototype, not the live site.** The intended replacement for `www.bolivarjesus.com`, currently on Wix — migration is decided, see `MIGRATION.md`. Marked `noindex` and linked from nowhere until it ships. |
| `feed.xml`, `sitemap.xml`, `robots.txt`, `og.png` | Feed, SEO, and the site-level social card |

## Design system

Shared with the essays: Newsreader for display, IBM Plex Sans for body, IBM Plex Mono for
labels and numbers; a light/dark palette driven by CSS custom properties. The landing page and
the essays share the same `localStorage` keys — `kp.lang` and `kp.theme` — so a reader's choice
of language and theme carries from the index into a piece and back.

## Adding a piece

1. Publish the piece under its own directory, with an `og.png` (1200×630) beside it.
2. In `index.html`, copy the `<article class="feature">` block in the *Writing* section, put the
   newest piece first, and add Spanish strings for its new `data-i18n` keys to the `ES` object
   at the bottom of the file. Every `data-i18n` key in the markup must have an `ES` entry;
   this is checked by eye, so grep before committing:
   ```sh
   python3 - <<'PY'
   import re; h=open('index.html').read()
   ks=set(re.findall(r'data-i18n="([^"]+)"',h))
   b=h[h.index('const ES = {'):h.index('};\nconst TR=')]
   print(sorted(ks-set(re.findall(r'"([A-Za-z0-9_.]+)"\s*:',b))) or 'all keys translated')
   PY
   ```
3. Add an `<item>` to `feed.xml` and a `<url>` to `sitemap.xml`.

## Local preview

```sh
python3 -m http.server 8000     # then open http://127.0.0.1:8000/
```

## Analytics

PostHog, cookieless: `persistence:'memory'`, no autocapture, no session recording, no person
profiles. Page views and page leaves, plus a handful of named events (language switch, theme
switch, piece opened).
