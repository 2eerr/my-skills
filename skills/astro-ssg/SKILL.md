---
name: astro-ssg
description: "Astro framework mechanics for building a data-driven static lead-gen site — project scaffold (JS + Tailwind, no TypeScript), astro.config.mjs, JSON data with getStaticPaths() routes, the Base.astro head wiring (canonical/OG/meta-robots/font preload/analytics), content-file rendering via a PageContent component, the component-first rule, dist/ output quirks, and post-build passes. Use when scaffolding, routing, templating, or building with Astro (or adapting the patterns to another SSG)."
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

## 2. Data-driven routes (never hand-write page files)

- All geography/services live in JSON (`src/data/*.json`); **`getStaticPaths()`** renders every
  route at build time. Pages are data × template, content is authored separately (below).
- Route file patterns (trailing-slash scheme per the technical-SEO skill):
  - `src/pages/index.astro` — home; `src/pages/[region].astro` — region page
  - `src/pages/[region]/[location].astro` — location page
  - `src/pages/[region]/[location]/[...page].astro` — money page (location×service)
  - `src/pages/services/[service].astro` — service hub; plus core pages (`/about/`, `/faq/`, …)
- Each route: look up the record from data, load its content file, and **gate publishing on the
  content** (see 4) — a route with no unique content must not render or ship.

## 3. Base layout & head wiring (`src/layouts/Base.astro`)

- One base layout for every page: `<title>`/meta description (overridable per page), absolute
  canonical built as `new URL(pathname, SITE.domain)` with pathname normalized to the trailing-
  slash scheme (root stays `/`); `og:url` and the `WebPage.url` JSON-LD node reuse that value.
- JSON-LD `@graph` wiring per the schema skill (base nodes in the layout, page nodes per route).
- `<meta name="robots">` + `referrer` per the technical-SEO skill; OG/Twitter defaults set here,
  pages override title/description only.
- Preload both self-hosted woff2 fonts with `fetchpriority="high"`; `font-display: swap` +
  fallback metrics so the swap causes zero CLS.
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
  plain inline `<script>` — respect `prefers-reduced-motion`.

## 6. Build output & post-build passes

- `npm run build` → `dist/` is the entire site; `public/` is copied in at build time.
- Astro emits `index.html` per route, so web servers serve both `/foo` and `/foo/` with no
  redirect rules — the canonical/trailing-slash scheme is enforced in code, not by the server.
- Post-build passes (npm scripts): sitemap generator (replaces `@astrojs/sitemap`; walks
  `dist/`, so only published pages appear) and a `fetchpriority` pass on the bundled CSS link.
- Dev parity: gates apply identically in `npm run dev` and production builds.

## Commands

```bash
npm create astro@latest -- --template minimal   # scaffold (JS, no TS)
npm run dev          # local render of routes + content gates
npm run build        # dist/ + post-build passes
npm run preview      # serve the built dist/ locally
```

## Checklist (done when)

- [ ] All routes come from JSON data via `getStaticPaths()` — zero hand-written page files.
- [ ] Base layout wires canonical/OG/JSON-LD/robots from `site.config.js` only.
- [ ] Content files render through `PageContent`; unpublished pages don't build.
- [ ] Every repeated UI block is a component; no inline copies.
- [ ] Build output: single CSS bundle (code-split off), sitemaps regenerated, `dist/` complete.

## Notes & gotchas

- Dynamic `import()` of content by path needs the project's helper — don't glob-load the whole
  content folder per page.
- A route that 404s in dev but exists in data usually means the content file is missing its
  `unique: true` line (the gate, not the router).
- Keep `cssCodeSplit: false` unless the deploy docs are updated together.
- Symlinks/paths with spaces (Windows): quote paths in npm scripts.
