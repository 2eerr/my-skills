---
name: astro-ssg
description: "Astro framework mechanics for a data-driven static lead-gen site — scaffold (JS + Tailwind, no TS), getStaticPaths() routes, Base.astro head wiring, content-file rendering via PageContent, component-first rule, dist/ output, post-build passes, and build performance at scale. Use when scaffolding, routing, or building with Astro or adapting patterns to another SSG."
metadata:
  internal: true
---

# Astro Static Site Mechanics

All Astro-specific instructions in one place: how the site is scaffolded, routed, templated,
rendered, and built. The `seo-*` skills own *what* content and rules go on pages; this skill owns
*how Astro builds them*. Read the project's config for brand/domain/paths — never hardcode.

## When to use it

- Scaffolding or restructuring an Astro project for a data-driven site.
- Adding/auditing routes, layouts, components, or content rendering.
- Debugging build output (routes, CSS bundles, post-build scripts).

## 1. Scaffold & config

- **Astro in plain JS — no TypeScript.** Add Tailwind CSS with the design-system tokens.
- `astro.config.mjs`: site URL from config; **`cssCodeSplit: false`** when the project relies on
  one shared CSS bundle (removing it re-introduces per-page bundles).
- `site.config.js` (single source of truth): brand, domain, NAP, hours, socials, analytics ID.
  Everything (canonicals, sitemap, robots, OG, JSON-LD) derives from it — changing the domain
  means updating it (+ the robots sitemap line) and rebuilding.
- Global CSS, self-hosted fonts, favicon + apple-touch-icon in `public/`.
- **Never gitignore `package.json`** — it is the project contract (deps, scripts, metadata).
- **Throwaway helpers go in `source/temp/`** (audits, one-off fixers): tracked in git, never packaged
  into the deploy zip, never deleted without the user asking. Real pipeline scripts live in
  `source/scripts/`.
- **No user-facing copy hardcoded in components** — all text lives in content files or data.

## 2. Data-driven routes (never hand-write page files)

- All geography/services live in JSON (`src/data/*.json`); **`getStaticPaths()`** renders every
  route at build time. Pages are data × template, content is authored separately (below).
- Route file patterns (trailing-slash scheme per the technical-SEO skill):
  - `src/pages/index.astro` — home; `src/pages/[region].astro` — region page
  - `src/pages/[region]/[location].astro` — location page
  - `src/pages/[region]/[location]/[...page].astro` — money page (location×service)
  - `src/pages/services/[service].astro` — service hub; plus core pages (`/about/`, `/faq/`, …)
  - Astro rest params must be **named** — `[...page].astro` is valid, bare `[...].astro` is not.
- Each route: look up the record from data, load its content file, and **gate publishing on the
  content** (see 4) — a route with no unique content must not render or ship.

## 3. Base layout & head wiring (`src/layouts/Base.astro`)

- One base layout for every page; it **implements** the rules owned elsewhere — title/meta
  patterns + OG/Twitter (`seo-onpage-seo`), canonical/robots values (`seo-onpage-seo` +
  `seo-technical-seo`), JSON-LD `@graph` (`seo-schema`), font loading (`seo-design-system`).
- Astro mechanics owned here:
  - Canonical built as `new URL(pathname, SITE.domain)` with pathname normalized to the
    trailing-slash scheme (root stays `/`); `og:url` and `WebPage.url` reuse that one value.
  - Page-level overrides flow through layout props/slots — pages set title/description/FAQs,
    never re-emit head tags.
  - Analytics script injected **only** when `import.meta.env.PROD` and the config ID is set.

## 4. Content-file rendering

- Page copy lives in `src/content/**` as standalone `.md` files (frontmatter `title`,
  `description`, `faqs`, `unique: true` + markdown body) — adding a page never needs a code or
  template change.
- Templates read the file when present and render the body through a shared **`PageContent`**
  component inside a `prose-brand` container (Tailwind Typography + brand tokens) so markdown
  maps to styled UI; fall back to template blocks only in dev — the **content gate**
  (a `hasPageContent()`-style helper checking `unique: true`) decides what builds/ships.
- FAQs render from frontmatter (accordion component) and feed `FAQPage` schema from the same
  source — never a second copy.

## 5. Components — component-first rule

- Every UI element used on more than one page **must** be a reusable component in
  `src/components/` — never copied inline into pages. If a block appears twice, extract it.
- Typical inventory: Header (mega menus, mobile overlay with focus management), Footer,
  HeroBand, SectionHeading, Breadcrumbs, TrustBar, ServiceGrid/LocationGrid/RegionGrid,
  StatsStrip, HowItWorks, Testimonials, FAQAccordion (`<details>/<summary>`), ContactCTA,
  StickyCTA, PageContent, ServiceIcon (slug → inline SVG map), Icon set.
- Conditional rendering in component frontmatter (e.g. menus hidden until content exists) —
  data-driven, no code changes when content ships.
- Keep pages static: client JS only for small interactions (menu toggle, scroll listeners) in
  plain inline `<script>` — motion/interaction rules belong to the design-system skill.

## 6. Build output & post-build passes

- `npm run build` → `dist/` is the entire site; `public/` is copied in at build time.
- Astro emits `index.html` per route, so web servers serve both `/foo` and `/foo/` with no
  redirect rules — the canonical/trailing-slash scheme is enforced in code, not by the server.
- Post-build passes (npm scripts): sitemap generator (replaces `@astrojs/sitemap`; walks
  `dist/`, so only published pages appear) and a `fetchpriority` pass on the bundled CSS link.
- Dev parity: gates apply identically in `npm run dev` and production builds.

## 7. Build performance at scale (hundreds of thousands of pages)

The content pipeline — not bundling — is the bottleneck at scale. Keep every mechanism below
intact; each one has a "don't revert" reason:

- **Lazy content reads** — the gate helper reads only the queried route's small frontmatter
  chunk (e.g. first 512 bytes), memoized in-process. Never an eager recursive scan of the whole
  content folder.
- **Persistent content-index cache** (e.g. `node_modules/.cache/…json`) — every hit validated
  by `statSync` (size + mtime) so content edits are always caught; written atomically on exit.
  Turns a ~90s full validation into ~2s warm builds. Force a re-scan by deleting the cache file.
- **Scope before completeness** — filter routes by build scope **before** running
  region/city-completeness checks, so a scoped build never touches the full content index.
- **Memoize hot data helpers** (completeness checks, visible-services, slugify) and serve
  lookups from pre-built `Map`s (`citiesIn(region)`, `locationsByRegion(slug)` — O(1)) instead
  of `.filter()` per call.
- **One shared markdown processor** instance across all renders — never re-created per page.
- **Parallelize post-build/gate scripts** (`fetchpriority` pass, SEO check, word-count check)
  with `worker_threads` capped at `max(1, min(16, cpus))`; don't depend on `UV_THREADPOOL_SIZE`.
- **Restrict Tailwind's `@source` scan to code dirs** and exclude the prose content folder —
  scanning tens of thousands of `.md` files is pure overhead on every cold start.
- **Keep templates light** — template cost multiplies across every rendered page; reuse shared
  components (rule 5) instead of inlining blocks per route.
- **One shared CSS bundle** (`cssCodeSplit: false`) — per-page CSS bundles explode at scale.
- **SVG-first imagery** until real photos exist (see the image-SEO skill).
- **Per-region sub-builds** when a full build is too slow: a `build:states`-style task sets a
  scope env var read by `getStaticPaths()` (same templates + data, filtered); batch 3–5 regions
  per run; never build the whole country in one go for iteration.
- **Regenerate sitemaps without a full rebuild** — a `gen:sitemaps`-style task walks the
  existing `dist/`; `<lastmod>` stays per-file (source mtime), never one build timestamp.
- Run every post-build pass (fetchpriority, sitemaps) on **all** build variants, including
  sub-builds and test builds.
- Build on **Node LTS** (20/22) — non-LTS releases show higher bundler variance.
- Scope note: runtime Core Web Vitals (LCP/INP/CLS, font preloading, critical CSS) belong to
  the technical-SEO skill — this section is about *build time* only.

## Commands

```bash
npm create astro@latest -- --template minimal   # scaffold (JS, no TS)
npm run dev          # local render of routes + content gates
npm run build        # prebuild → Astro build → fetchpriority → sitemaps
npm run preview      # serve the built dist/ locally
```

### Build pipeline scripts

| Script | Location | Runs when | Purpose |
|---|---|---|---|
| `prebuild-check.js` | `source/scripts/` | `prebuild` hook | Clears stale content cache, logs content counts, verifies `unique: true` |
| `build.js` | `source/scripts/` | `npm run build:states` | Scoped build via `BUILD_STATES` env var for `getStaticPaths()` |
| `add-fetchpriority.js` | `source/scripts/` | Post-build | Adds `fetchpriority="high"` to stylesheet `<link>` tags; parallelized with `worker_threads` |
| `generate-sitemaps.js` | `source/scripts/` | Post-build | Walks `dist/`, produces `sitemap-index.xml` + per-region child sitemaps from source file `mtime` |
| `check-seo.js` | `source/scripts/` | `npm run check:seo` | Scans built HTML for missing SEO tags |
| `check-wordcounts.js` | `source/scripts/` | `npm run check:wordcounts` | Verifies word counts per page type |
| `validate-data.js` | `source/scripts/` | `npm run validate` | Validates JSON data files |
| `gen-pages-tracker.js` | `source/scripts/` | `npm run gen:tracker` | Regenerates `TRACKER.md` from content |
| `package-deploy.js` | `source/scripts/` | `npm run zip` | Builds deploy.zip source archive |

## Checklist (done when)

- [ ] All routes come from JSON data via `getStaticPaths()` — zero hand-written page files.
- [ ] Base layout wires canonical/OG/JSON-LD/robots from `site.config.js` only.
- [ ] Content files render through `PageContent`; unpublished pages don't build.
- [ ] Every repeated UI block is a component; no inline copies.
- [ ] Build output: single CSS bundle (code-split off), sitemaps regenerated, `dist/` complete.
- [ ] Full build completes in acceptable time; sub-build + sitemap-only paths available.

## Notes & gotchas

- Dynamic `import()` of content by path needs the project's helper — don't glob-load the whole
  content folder per page.
- A route that 404s in dev but exists in data usually means the content file is missing its
  `unique: true` line (the gate, not the router).
- Keep `cssCodeSplit: false` unless the deploy docs are updated together.
- Symlinks/paths with spaces (Windows): quote paths in npm scripts.
