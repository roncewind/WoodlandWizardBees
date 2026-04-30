# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Jekyll-based GitHub Pages blog ("Amberfold Bees") for beekeeping hive notes. There is no application code — only Jekyll content and config. The site uses the `minima` theme (see `_config.yml`). GitHub Pages itself builds and deploys on push; there is no site CI in this repo.

## Mental model

Three entities:

1. **Apiary** — a yard / grouping of hives. Currently `Orchard`, `Waymeet` (the driveway), `Underhill` (the down-the-hill yard, dormant since spring 2025). Each apiary has a two-letter prefix: `OR`, `WM`, `UH`. Registered in `_data/apiaries.yml`.
2. **Hive position** — a specific spot within an apiary. Permanent ID `<apiary-prefix>-<NN>` (`OR-01`, `OR-02`, `WM-01`, …). IDs are forever and never reused, even after a position is retired (`status: retired`). Registered in `_data/hives.yml`. There is no `_hives/` collection — these are field-only.
3. **Colony** — the biological lineage living at a hive. Permanent ID `C-YYYY-NN` (year first observed, then sequence). Tracked in `_colonies/<id>.md`. Splits/swarms record `parent: <colony-id>` so lineage walks backward in one hop.

A colony references its current hive via the `hive:` front-matter field (e.g. `hive: OR-02`). Inspection posts can carry their own `hive:` for the rare day a colony was somewhere different — otherwise the inspection layout falls back to `colony.hive`. So in practice: when a colony moves, update its `hive:` field once on the colony page, and most posts need no override.

Other notes on the lineage / queen model:

- Even after a colony dies, its file stays as a historical record (`status: deadout` + `end_date`).
- Splits/swarms record `parent: <colony-id>` so lineage walks backward in one hop. The colony layout queries forward (`site.colonies | where: "parent", page.slug`) to render a "Descendants" list, so the family tree is implicit.
- Combines (two parents → one child) aren't expressible with a single `parent:` field. If they happen, either widen `parent` to a list or capture it in prose for now and revisit.
- Queen identity is recorded as front-matter fields on the colony (`queen_id`, `queen_origin`, `queen_color`). When a queen is replaced, update those fields — there's no separate `_queens/` collection yet.

## Authoring posts

The repo has two coexisting post styles:

**New structured format** (use for new entries):

- Filename: `_posts/YYYY-MM-DD-<colony-id>-inspection.md` (e.g. `2026-04-29-C-2025-01-inspection.md`).
- Front matter: `layout: inspection`, `colony_id:` (must match a `_colonies/<id>.md` slug), plus optional `location`, `queen_status`, `temperament`, `brood`, `stores`, `actions:` (list), `tags:` (list). Every field is conditional in the layout, so partial fills render cleanly.
- Body is prose. Front matter is for filtering and aggregation; the writing is for thinking.
- Multiple inspections on the same day are fine — the colony id disambiguates the filename.
- The fastest way to start one is `bin/new-inspection <colony-id>`. The script validates the colony id against `_colonies/`, refuses to clobber an existing file, and lists available colonies if called with no args or a typo.

**Legacy posts** (pre-2026): no `layout` field, plain prose with `title` + `date` only. They render via minima's default `post` layout and should be left alone — don't retrofit them unless asked.

## Colonies collection

`_colonies/<C-YYYY-NN>.md` defines each colony. Files render via `_layouts/colony.html` at `/colonies/<id>/`, showing the metadata block plus a reverse-chron list of inspections (`site.posts | where: "colony_id", page.slug`) and a "Descendants" list (other colonies whose `parent:` matches this slug).

Front-matter fields used by the layout:

- `title` (display name — usually just the id), `slug` (must equal the filename basename; set explicitly to be safe).
- `status` (`active` | `deadout` | `combined` | `requeened` | `split`), `end_date` (when status leaves `active`).
- `origin` (`nuc` | `package` | `swarm` | `split` | `combine`), `established` (date the colony entered the records).
- `location` (current — free text; old locations belong in prose or in past inspection posts).
- `queen_id`, `queen_origin`, `queen_color` (current queen).
- `parent` (a colony slug) — sets the lineage edge.

`_data/colonies.yml` holds a small parallel index. It's not required by any layout right now — keep it in sync only if you start using it for cross-cutting lists.

## Templates and layouts

- `_layouts/inspection.html` extends `post` and prepends a stats block from front matter.
- `_layouts/colony.html` extends `page`, renders the colony metadata, and lists inspections + descendants. Both `where` filters rely on the explicit `slug:` field in colony front matter, so don't drop it.
- `_templates/inspection.md` is the canonical front-matter template. It lives under `_templates/` (not `_drafts/`) so Jekyll won't accidentally publish it — any underscore-prefixed dir that isn't a known Jekyll collection is excluded automatically.

## Site URL prefix

The published site lives under the path `/AmberfoldBees/` (matching the GitHub repo name). When linking to assets from Markdown, include that prefix — relative `/assets/...` paths will 404 on the deployed site. If the repo is ever renamed again, every hardcoded `/AmberfoldBees/...` link in posts needs to follow; consider switching to Jekyll's `{{ "/path" | relative_url }}` filter to avoid that.

## Local preview (optional)

Standard Jekyll workflow if you have Ruby/Bundler installed:

```
bundle exec jekyll serve
```

There is no `Gemfile` checked in, so `bundle init && bundle add jekyll minima` is needed first. GitHub Pages handles the actual build on push, so local preview is not required for publishing.
