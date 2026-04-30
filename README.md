# Amberfold Bees

A field notebook for the bees in our yards — Jekyll-based, hosted on GitHub Pages.

Live site: <https://roncewind.github.io/AmberfoldBees/>

## What's tracked

Three things, kept separate on purpose:

- **Apiaries** — the yards (`Orchard`, `Waymeet`, `Underhill`). Indexed in `_data/apiaries.yml`.
- **Hive positions** — physical spots within a yard, with permanent IDs like `OR-01`, `WM-02`. Indexed in `_data/hives.yml`. IDs are never reused.
- **Colonies** — the biological lineage living at a hive. Permanent IDs like `C-2025-01`. Each colony is a page under `_colonies/`, with current queen, status, and a link back to its parent colony if it came from a split or swarm.

A colony's `hive:` field says where it lives now. When a colony moves, that field is the only thing that needs updating.

## Publishing

Push to `main`. GitHub Pages builds and deploys on its own — usually live within a minute or two. There's no separate CI step.

## Adding an inspection (a post)

```
bin/new-inspection C-2025-01
```

That scaffolds `_posts/YYYY-MM-DD-C-2025-01-inspection.md` with the right front matter. Fill in what you saw, commit, push.

The post layout (`_layouts/inspection.html`) renders any front-matter fields you fill in (queen status, temperament, brood, stores, actions taken) and then your prose. Partial fills are fine; empty fields just don't render. The colony id in the filename is what links the post into the colony's inspection history.

## Adding a new colony

A colony is a page, not a post. Create `_colonies/<id>.md` where `<id>` follows `C-YYYY-NN` (year first observed, next sequence number). For example, the third colony first seen in 2026 would be `_colonies/C-2026-03.md`.

Minimum front matter:

```yaml
---
title: C-2026-03
slug: C-2026-03
status: active
origin: swarm        # nuc | package | swarm | split | combine
established: 2026-05-12
location: Orchard
hive: OR-02
queen_origin: caught swarm
queen_color: unmarked
parent: C-2025-02    # only if it came from a split/swarm of an existing colony
---

Prose about the colony goes here.
```

`slug:` must match the filename basename — the colony layout's "Descendants" lookup depends on it. After saving, the page lives at `/colonies/C-2026-03/`, and any inspection post tagged with `colony_id: C-2026-03` will show up on it automatically.

## Adding a top-level page

Drop a Markdown file at the repo root (e.g. `about.md`) with front matter:

```yaml
---
layout: page
title: About
permalink: /about/
---
```

Then write the page body below. It'll show up at `/about/` after the next push. The minima theme will link top-level pages in the site header automatically.

## Browsing

- `/` — reverse-chron post feed across all colonies
- `/colonies/<id>/` — colony page with its inspection history and any descendant colonies

## Local preview (optional)

Standard Jekyll. There's no `Gemfile` checked in, so:

```
bundle init && bundle add jekyll minima
bundle exec jekyll serve
```

Not required for publishing — GitHub Pages handles the build.

# Asset building

## 🏷️ 🐝 AMBERFOLD LABEL TYPOGRAPHY SYSTEM

This is tuned for a standard 8oz–16oz honey jar front label (~3.5–4.5 inches wide)

⸻

🟢 1. Primary Logo (AMBERFOLD BEES)

Font: Playfair Display
Weight: Bold (700)

Settings:

Font size: 42–52 pt
Tracking: +30
Line height: 90% (tight)
Case: ALL CAPS
Alignment: Center

Layout:

AMBERFOLD
   BEES

👉 "BEES" should be ~40–50% the size of "AMBERFOLD"

Example:

* AMBERFOLD → 48 pt
* BEES → 20–24 pt

⸻

🌿 2. Tagline (Raw • Local • Unfiltered)

Font: Lora
Weight: Regular (400)

Settings:

Font size: 12–14 pt
Tracking: +80
Line height: 120%
Case: Small caps (or ALL CAPS if needed)
Alignment: Center

Style:

Use separators like:

RAW • LOCAL • UNFILTERED

👉 The spacing is what makes this feel premium. Don't tighten it.

⸻

🍯 3. Product Name ("RAW HONEY")

This is your secondary focal point

Font: Playfair Display
Weight: Bold or SemiBold

Settings:

Font size: 22–28 pt
Tracking: +20
Case: ALL CAPS
Alignment: Center

👉 Slightly smaller than the main logo, but still strong

⸻

🧾 4. Supporting Line (Pure • Local • Unfiltered)

Font: Lora
Weight: Regular

Font size: 10–12 pt
Tracking: +60
Alignment: Center

⸻

⚖️ 5. Net Weight (tiny but important)

Font: Lora Regular
Font size: 8–9 pt
Tracking: +40
Alignment: Center

Example:

NET WT. 1 LB (16 OZ) 454g

⸻

🌼 6. Floral / Decorative Balance

Spacing matters as much as fonts:

Vertical spacing (use this rhythm):

Top margin → 0.4 in
Logo → gap → 0.15 in
Tagline → gap → 0.25 in
Illustration (door/floral)
→ gap → 0.25 in
RAW HONEY
→ gap → 0.15 in
Supporting text
→ gap → 0.2 in
Net weight (bottom)