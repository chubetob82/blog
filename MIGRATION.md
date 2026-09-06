# Moving www.bolivarjesus.com off Wix — what it would take

Working notes, not a decision. `www-preview/index.html` in this branch is the evidence: the
whole of the current Wix homepage, rebuilt as one hand-written HTML file in the blog's design
system. Open it next to the live site and judge.

## What is on Wix today

From the Wix account and the live page:

- **Site**: "Jesus Bolivar", Premium plan, custom domain, classic **Editor** (not Velo), created
  March 2024, last updated June 2026.
- **Page**: a one-page CV — hero, *My story*, *Experience*, *Education and training*,
  *Skills & Languages*, *Awards*, *Interests*, *Publications*, *Contact*. A "More" item in the nav
  suggests at least one page beyond the homepage.
- **Apps installed**: Promote SEO, **Wix Bookings**, **Wix Invoices**, Wix Portfolio.

Everything visible on the homepage is static text and images. Nothing on it needs a server.

## The one question that decides it

**Are Wix Bookings and Wix Invoices actually in use?**

- If they are dormant (installed once, never used): the site is pure content, and everything
  Wix is doing for it, GitHub Pages does for free. Migrate.
- If real people book time or receive invoices through them: those are applications, not pages.
  Static HTML cannot replace them, and replacing them separately (Cal.com, Stripe Invoicing,
  a bookkeeping tool) is a bigger project than moving a homepage. Keep Wix, or migrate the
  homepage and move those two workflows to purpose-built tools first.

Nothing else on the site is a blocker.

## What changes if you migrate

| | Wix today | Static on GitHub Pages |
| --- | --- | --- |
| Cost | Premium subscription, annual | $0 hosting; you still pay for the domain |
| Editing | Wix editor | Edit HTML, commit, push — same loop as the blog |
| Contact | Wix form | `mailto:` and WhatsApp links (what the preview does), or a third-party form endpoint if you want a real form |
| Page weight | ~640 KB of Wix runtime on the homepage | ~20 KB of HTML plus the images |
| Design | Wix theme | The same system as the essays — one identity across both domains |
| Bookings / invoices | Built in | Not available; needs separate tools |

## Mechanics

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

## Recommendation

If Bookings and Invoices are dormant: migrate. The content is small, static, and already
rebuilt; the two domains would finally look like one person's work; and the maintenance loop
becomes the one you are already using for the essays.

If either is live: leave `www` on Wix for now, and revisit once those workflows have moved to
tools built for them.
