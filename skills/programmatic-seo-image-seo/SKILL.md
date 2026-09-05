---
name: programmatic-seo-image-seo
description: "Image rules for a programmatic local-SEO lead-gen site (any niche) — self-hosting, formats (SVG icons/illustrations, WebP photos), width/height + alt + lazy-loading, unique-per-page imagery, the OG social card, and the freely-licensed image attribution practice. Use when adding, optimizing, or auditing any image or icon on the site."
metadata:
  internal: true
---

# Programmatic SEO — Image SEO & Licensing

All image rules in one place: performance, accessibility, SEO, and licensing. Applies whether
the site renders photos now or adds them later. Component styling of images lives in the
design-system skill; this skill owns the image practice itself.

## 1. Hosting & formats

- **Self-host** everything under the site's images folder (e.g. `public/images/`) — no external
  CDNs, no hotlinking.
- **SVG** for icons and illustrations (crisp, tiny, no layout shift); **WebP** for photos.
- Icons are always real SVG (or another graphic file) — **never emoji, dingbats, or text
  glyphs** (`★ → ⚠ ✓ ✕`) as visible UI elements.

## 2. Every `<img>` (hard gate)

- `width` + `height` declared and **matching the file's intrinsic size** (center-crop source
  images to the exact display size so nothing is served cropped-off or shifted).
- Descriptive `alt`: keyword + context, not stuffed; decorative images get `alt=""`.
- `loading="lazy"` below the fold; eager for the above-fold hero only.
- Optimized/compressed; unique images per page type (vary by region where feasible) — no one
  stock photo repeated across sibling pages.

## 3. Standard sizes (tune per project)

| Asset | Size | Ratio |
|---|---|---|
| Section/hub photo | 1200×675 | 16:9 |
| About photo | 1200×900 | 4:3 |
| OG social card | 1200×630 | 1.91:1 |

- The OG card ships with `og:image:width/height/alt` + `twitter:image:alt` (tag wiring is in the
  on-page skill); an original illustration is fine as the default card.
- Prefer lightweight SVG illustrations until real photos are available — keeps huge static
  builds fast.

## 4. Licensing & attribution

- Only **freely licensed or public-domain** photos (public domain / CC BY / CC BY-SA) from
  verified sources; original artwork needs no license.
- Keep an **attribution table** (file → source article/URL → author → license) in the project
  docs and keep it accurate; fill exact author/license per file, not "see source".
- If images are temporarily removed from pages, **keep the attribution list** so re-adding is
  safe.
- A small fetch script may download/convert/crop licensed assets to the sizes above — scripts
  prepare files, never invent content.

## Checklist (done when)

- [ ] All assets self-hosted; icons/illustrations SVG, photos WebP.
- [ ] Every `<img>`: intrinsic-matching width/height + meaningful alt + correct lazy/eager.
- [ ] No emoji/text-glyph icons anywhere in UI or content.
- [ ] OG card 1200×630 with width/height/alt tags set.
- [ ] Every photo's license verified and recorded in the attribution table.
