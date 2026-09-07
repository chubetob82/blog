# Moving www.bolivarjesus.com off Wix

**Decided: migrate.** Bookings and Invoices turned out to be dormant, which was the only thing
that could have stopped this. `www-preview/index.html` in this branch is the destination — the
whole of the current Wix homepage, rebuilt as one hand-written HTML file in the blog's design
system. What follows is the sequence to get there.

## What is on Wix today

From the Wix account and the live page:

- **Site**: "Jesus Bolivar", Premium plan, custom domain, classic **Editor** (not Velo), created
  March 2024, last updated June 2026.
- **Page**: a one-page CV — hero, *My story*, *Experience*, *Education and training*,
  *Skills & Languages*, *Awards*, *Interests*, *Publications*, *Contact*. A "More" item in the nav
  suggests at least one page beyond the homepage.
- **Apps installed**: Promote SEO, **Wix Bookings**, **Wix Invoices**, Wix Portfolio.

Everything visible on the homepage is static text and images. Nothing on it needs a server.

## The question that decided it — settled

**Are Wix Bookings and Wix Invoices actually in use?** They are not. Both were installed and
never used.

That was the only real blocker. They are applications, not pages: static HTML cannot replace a
booking calendar or an invoicing ledger, and moving those workflows to purpose-built tools
would have been a bigger project than moving a homepage. Dormant, they cost nothing to leave
behind.

Everything else on the site is static text and images. Nothing on it needs a server, so
everything Wix is doing for this domain, GitHub Pages does for free.

One thing to confirm before you cancel rather than before you start: that neither app holds
records worth keeping — a stray test booking, an invoice someone actually received. Cancelling
takes the data with it. I can check both through the Wix connector if you want it done
properly.

## What changes if you migrate

| | Wix today | Static on GitHub Pages |
| --- | --- | --- |
| Cost | Premium subscription, annual | $0 hosting; you still pay for the domain |
| Editing | Wix editor | Edit HTML, commit, push — same loop as the blog |
| Contact | Wix form | `mailto:` and WhatsApp links (what the preview does), or a third-party form endpoint if you want a real form |
| Page weight | ~640 KB of Wix runtime on the homepage | ~20 KB of HTML plus the images |
| Design | Wix theme | The same system as the essays — one identity across both domains |
| Bookings / invoices | Built in | Not available; needs separate tools |

## Mechanics, in detail

1. **A second repository.** A GitHub Pages site can carry only one custom domain in its `CNAME`
   file, and this one is spoken for by `blog.bolivarjesus.com`. So `www` needs its own repo,
   with `CNAME` containing `www.bolivarjesus.com`. Folding both into one repo would mean moving
   the essays to `/blog/...`, which breaks the URLs already published — don't.
2. **DNS.** Point `www` at the new repo's Pages site with a `CNAME` record to
   `<user>.github.io`, and point the apex `bolivarjesus.com` at the GitHub Pages A records so
   the bare domain redirects. Both cut over independently of `blog`, which keeps working
   untouched throughout.
3. **Images.** The photos currently live on `static.wixstatic.com`. Download them into the repo
   before cutting DNS — once the Wix subscription lapses those URLs may stop resolving.
   `portrait.jpg` in this repo was pulled that way.
4. **Redirects.** List the Wix page URLs (the "More" pages, anything shared or linked from
   elsewhere) and give each a landing spot in the new site, or a small redirect page. Wix's own
   redirects disappear with the subscription.
5. **Keep the Wix site parked** for a month after cutover rather than cancelling immediately,
   so nothing is lost if something turns out to have been living there.

## What the preview is missing

`www-preview/index.html` is a complete homepage, but if it becomes the real site it still wants:

- The Spanish version. The blog does EN/ES in one file with a toggle; the same pattern applies
  here, and doing it would put the personal site ahead of where Wix was.
- Its own `og.png` social card, favicon, `sitemap.xml`, and analytics snippet.
- Whatever lives behind the "More" nav item on the current site — I could only read the homepage.
- A decision on the contact form (`mailto:` may be enough).

## The sequence

Ordered so that nothing is live and broken at any point, and `blog.bolivarjesus.com` is never
touched:

1. **Finish the preview** — the list above. The Spanish version is the one that takes real work;
   the rest is an afternoon.
2. **New repository**, `CNAME` = `www.bolivarjesus.com`, the preview as its `index.html`.
   Publish it on the `github.io` URL first and live with it for a few days.
3. **Pull the remaining assets off `static.wixstatic.com`** — every image on every page, not just
   the homepage — and commit them.
4. **Inventory the Wix URLs**, including whatever sits behind "More", and give each one a
   destination or a redirect.
5. **Cut DNS**: `www` to the new Pages site, apex to the GitHub A records. Watch it for a week.
6. **Park Wix for a month**, then cancel. Check Bookings and Invoices for records before you do.

The two domains end up looking like one person's work, and the maintenance loop for `www`
becomes the one you are already using for the essays.
