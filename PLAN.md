# Site Redesign Plan

Tracks implementation progress for the Calgary Aikikai Hugo/Blowfish site.
Work through phases in order. Check off items as they're completed.

---

## Phase 1 — Global Config (high visual impact, do first)

- [ ] Enable `showCards = true` in `params.toml` list section so blog/seminar listings render as image cards
- [ ] Set default `heroStyle` in `params.toml` (suggest `"big"` as the global default)
- [ ] Enable `showHero = true` in `params.toml` article section
- [ ] Enable YouTube privacy mode in `hugo.toml`: `[privacy.youtube] privacyEnhanced = true`
- [ ] Verify Fuse.js search index is working (Blowfish search enabled in config)

---

## Phase 2 — Schedule Page

Goal: eliminate raw HTML editing — all schedule changes via YAML only.

- [ ] Audit `data/schedule.yaml` — ensure structure matches `day / classes[]` with `time`, `name`, `level` fields
- [ ] Create `layouts/partials/schedule-table.html` that loops over `site.Data.schedule`
- [ ] Replace `{{< rawhtml >}}` block in `content/schedule/_index.md` with partial call
- [ ] Add colour-coding by class level (Kids vs Adults vs Weapons) via CSS class on `level` field
- [ ] Test: edit one entry in YAML, verify it updates the rendered page

---

## Phase 3 — Blog Posts

Goal: page bundles with feature images, hero treatment, photo galleries for event recaps.

- [ ] Convert existing blog posts to page bundles (`post-name/index.md` + `feature.jpg`)
- [ ] Add a `feature.jpg` to at least the most recent 5–10 posts as a starting point
- [ ] Set `heroStyle: "big"` as default; use `"background"` for major event posts
- [ ] Enable `groupByYear = true` on `content/blog/_index.md` for auto-archiving
- [ ] Add `{{< gallery >}}` shortcode to event recap posts that have multiple photos
- [ ] Verify listing page renders cards with thumbnails

---

## Phase 4 — Seminar Pages

Goal: strong detail pages with hero images, gallery for recaps, register button.

- [ ] Convert seminar detail pages to page bundles (`seminars/event-name/index.md` + `feature.jpg`)
- [ ] Add `heroStyle: "background"` to upcoming seminars, `"big"` to past
- [ ] Add Register button to detail pages: `{{< button href=".Params.square_link" >}}Register Now{{< /button >}}`
- [ ] Add `{{< gallery >}}` to past seminar pages that have recap photos
- [ ] Add optional `{{< youtube >}}` shortcode for seminar promo videos
- [ ] Set up nightly Netlify rebuild so upcoming/past split stays accurate without manual deploys
  - Add `@netlify/plugin-cron` to `netlify.toml` with `0 6 * * *` schedule

---

## Phase 5 — Pricing Page

Goal: tabbed layout by program type, clear CTA to Square store.

- [ ] Restructure `content/pricing/_index.md` using `{{< tabs >}}` / `{{< tab >}}` shortcodes
  - Tabs: Adults | Kids & Youth | Drop-in & Trials
- [ ] Add `{{< lead >}}` intro sentence above tabs
- [ ] Add `{{< button href="https://store.calgaryaikikai.com" >}}` CTA in each tab
- [ ] Add `{{< alert >}}` for any pricing notes (e.g. "Pricing reviewed annually")

---

## Phase 6 — Beginners & Kids Pages

Goal: scannable, mobile-friendly, clear class times, strong CTA.

- [ ] Add `{{< lead >}}` opener to `content/beginners/_index.md`
- [ ] Add `{{< alert >}}` with current beginner class day/time
- [ ] Add `{{< button >}}` CTA linking to Schedule and Contact
- [ ] Add feature image + `heroStyle: "big"` to beginners page
- [ ] Mirror same structure for `content/kids/_index.md`
- [ ] Add `{{< tabs >}}` to kids page if splitting Kids (5–8) vs Youth (9–14) is useful

---

## Phase 7 — About Page

Goal: engaging dojo history with timeline, instructor profiles from data file.

- [ ] Replace history prose in `content/about/_index.md` with `{{< timeline >}}` / `{{< timelineItem >}}` shortcodes
  - Key milestones: founding (1980), Inaba Shihan relationship, notable gradings, current era
- [ ] Audit `data/instructors.yaml` — ensure all instructors have name, rank, bio, photo fields
- [ ] Add headshot images to `static/images/instructors/` for each instructor
- [ ] Create `layouts/partials/instructor-cards.html` that loops over `site.Data.instructors`
- [ ] Replace any hardcoded instructor HTML in `about/_index.md` with partial call

---

## Phase 8 — Aikido Page

Goal: philosophy/marketing page with strong visual entry point and CTAs.

- [ ] Add hero image + `heroStyle: "background"` to `content/aikido/_index.md`
- [ ] Add `{{< lead >}}` opening statement
- [ ] Add `{{< button >}}` CTAs → Schedule and Beginners pages at the bottom

---

## Phase 9 — Polish & Infrastructure

- [ ] Create `layouts/404.html` — branded error page with nav links
- [ ] Create `static/_redirects` to preserve old WordPress URL paths
- [ ] Audit taxonomy pages (tags/categories) — ensure `showCards = true` applies
- [ ] Add Slack community link and Google Forms testing application link to Contact or nav
- [ ] Review all `pages/` section pages — confirm archetype front matter is applied consistently
- [ ] Run Lighthouse / PageSpeed audit and address any major issues

---

## Phase 10 — Video (optional, do when relevant)

- [ ] Create `layouts/shortcodes/youtube-channel.html` for build-time RSS channel feed
  - Fetches latest N videos from YouTube channel RSS — no API key needed
  - Optionally add a "Recent Videos" section to homepage or a dedicated page
- [ ] Evaluate whether `lite-youtube-embed` is needed for performance (likely not for low-traffic dojo site)

---

## Notes

- **Image workflow:** Resize photos to max 1600px wide, convert to WebP at 80% quality using [Squoosh](https://squoosh.app) before committing
- **Google Photos:** Do not embed — link to shared albums in blog posts instead
- **YouTube single video:** `{{< youtube VIDEO_ID >}}` (built-in shortcode, privacy mode enabled in Phase 1)
- **Photo galleries:** `{{< gallery >}}` with images as page bundle siblings of `index.md`
- **Seminar register button:** requires `square_link` field in seminar front matter
