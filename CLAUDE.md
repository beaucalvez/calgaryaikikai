# Calgary Aikikai Hugo Site

## Project
Convert calgaryaikikai.com from WordPress (Tersus theme) to Hugo static site using Blowfish theme. Site is for an aikido dojo established 1980 in Calgary, AB. Primary purpose: marketing, class info, event/seminar listings, blog posts. Single technical maintainer.

## Stack
- SSG: Hugo (extended)
- Theme: Blowfish (MIT, github.com/nunocoracao/blowfish)
- CSS: Tailwind (via Blowfish)
- Hosting: Netlify or Cloudflare Pages
- Registration: Mailchimp (external link, nav item links out)
- Store: Square (external link to store.calgaryaikikai.com)
- Optional CMS: Decap CMS (Git-based)
- Forms: Netlify Forms or Formspree

## Site Structure
```
content/
├── _index.md              # homepage
├── about/
│   └── _index.md          # dojo history, lineage, instructors
├── schedule/
│   └── _index.md          # weekly class schedule
├── blog/
│   ├── _index.md
│   └── [posts].md
├── seminars/
│   ├── _index.md          # listing: upcoming/past auto-split by date
│   └── [events].md
├── pricing/
│   └── _index.md
├── beginners/
│   └── _index.md
├── kids/
│   └── _index.md
├── registration/
│   └── _index.md          # registration info + external Mailchimp link
├── pages/
│   ├── dojo-calendar.md
│   ├── testing-requirements.md
│   ├── vocabulary.md
│   ├── rei-and-bowing.md
│   ├── teachers-notes.md
│   └── [other dojo info pages].md
└── contact/
    └── _index.md
```

Note: `about/` consolidates History of Calgary Aikikai, Inaba Shihan, and Instructors content from WP. The `pages/` section handles standalone Dojo Info sidebar pages that don't belong elsewhere.

## Page Archetype (archetypes/pages.md)
```yaml
---
title: "{{ replace .Name \"-\" \" \" | title }}"
date: {{ .Date }}
draft: true
summary: ""
showDate: false
showReadingTime: false
showPagination: false
---
```
Usage: `hugo new pages/vocabulary.md`

## Seminar Archetype (archetypes/seminars.md)
```yaml
---
title: "{{ replace .Name \"-\" \" \" | title }}"
date: {{ .Date }}
draft: true
instructor: ""
instructor_bio: ""
rank: ""                    # e.g. "6th Dan Shihan"
start_date: {{ .Date }}
end_date: {{ .Date }}
times: ""                   # e.g. "Saturday 10am–4pm, Sunday 10am–2pm"
location: "Calgary Aikikai"
address: ""
price: ""                   # e.g. "$120 both days / $75 single day"
square_link: ""             # link to Square store item
summary: ""
image: ""
tags: []
---
```
Usage: `hugo new seminars/2026-fall-weapons.md`

## Seminar Listing Logic
Template at `layouts/seminars/list.html` should split into upcoming vs past using `start_date` compared to `now`. Upcoming sorted ascending, past sorted descending.

## Data Files
- `data/schedule.yaml` — weekly class schedule (day, time, class name, level)
- `data/instructors.yaml` — instructor name, rank, bio, photo

## Key Pages from Current WP Site
Nav: Home, Aikido, Pricing, Schedule, Seminars, Contact, Registration (external → Mailchimp)
Dojo Info sidebar pages (→ `pages/` section):
  - Beginners Classes, Kids Programme, Dojo Calendar, History of Calgary Aikikai,
    Inaba Shihan, Instructors, Kids/Adults/Beginners Programs, Seminars,
    Teacher's Notes, Testing Requirements, Vocabulary, 礼 Rei and Bowing in Aikido
Forms & Community sidebar: Slack invite, Google Forms testing application
Blog categories: Adult news, Demo, Events, Kids news, News, Perspectives, Promotions, SeminarPost, Seminars, Testing, Training
External links: Square store (store.calgaryaikikai.com), Slack community, Google Forms testing application

## Migration Notes
- Export WP content with wordpress-to-hugo-exporter or wp2hugo
- Clean up image paths, shortcodes, HTML artifacts
- Set up redirects (_redirects file) to preserve old URL paths
- Optimize images before adding to static/images/
- Current site has posts back to 2014; archive content in blog section

## Blowfish Config
Config lives in `config/_default/` with files: hugo.toml, params.toml, languages.en.toml, menus.en.toml, markup.toml
Theme installed as git submodule or Hugo module. CLI available: `npx blowfish-tools`

## Design Direction
Clean, minimal, content-focused. Match the calm/traditional aesthetic appropriate for an aikido dojo. Light default with dark mode option. Use Blowfish's built-in color schemes or create custom one.