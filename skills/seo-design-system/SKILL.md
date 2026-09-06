---
name: seo-design-system
description: A premium, trust-building UI/design system for a local-SEO lead-gen site (any niche) — brand voice, a token-based color system, typography pairing, spacing & layout, radius/shadows, core components, accessibility rules, and an image attribution/licensing practice. Use when building or styling site components and pages so content renders as a polished product page.
metadata:
  internal: true
---

# SEO Design System

Single source of truth for look & feel. Content is rendered through these styles, so pages must
render as a premium product page, never a pasted document. Tokens are theme-able — set the
brand palette per project; the structure below is niche-agnostic.

## Brand

- **Name:** the project's brand (from config, e.g. `site.config.js`).
- **Personality:** premium, bold, modern, trustworthy — national brand, local trust.
- **Copy tone:** helpful, confident, local, plain-English, short sentences, no fear-mongering.
- **Logo:** simple mark + wordmark; provide a light/dark variant and favicon + apple-touch-icon.

## Color tokens (theme-able "premium bold" direction)

Define semantic tokens, not raw hexes, so any brand can drop in:

| Token | Role |
|---|---|
| `--color-primary` / `-dark` / `-bright` / `-soft` | brand color: buttons, links, active, borders; gradient/hover; glows/dark-surface accents; section tints |
| `--color-accent` / `-soft` | high-attention CTA, stars, urgency badges (use sparingly) |
| `--color-ink` / `-body` / `-muted` | primary text / secondary text / dark-band secondary text |
| `--color-line` | borders, dividers |
| `--color-surface` / `-soft` | cards/page bg / alternating sections |
| `--color-navy` / `-2` / `--color-footer` | dark header, heroes, bands, footer |

**Contrast rules:** ink/body on light ≥4.5:1. Accent color only for non-essential accents, never
body text. Pick text color on accent by measured contrast (not by habit). On dark surfaces use
`--color-muted` or brighter, never mid-grays. Primary text on dark heroes is white.

## Typography

- **Headings:** a distinctive geometric/grotesk sans (e.g. Plus Jakarta Sans, Sora, Manrope),
  weights 600–800, tight leading (`~1.05`), `tracking-tight`, sentence case.
- **Body:** a neutral UI sans (e.g. Inter), 400/500/600, `text-base`, `leading-relaxed`.
- **Scale (rem):** H1 ~3.5/4/hero 6 · H2 2.25/3 · H3 1.5 · H4 1.25 · body 1 · small .875 · micro .75.
- Self-host woff2, preload with `fetchpriority="high"`, add `ascent-override`/`descent-override`
  (+ `size-adjust`) so the fallback→web-font swap causes zero layout shift.

## Spacing, radius, shadows

- Base unit `0.25rem`; container `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8`.
- Section rhythm `py-16 lg:py-24`; 12-col grids; cards `grid-cols-1 sm:2 lg:3 xl:4`.
- Section heading rhythm: overline (small uppercase) → title (H2) → sub (muted lede).
- Radius: buttons/inputs `rounded-xl`, cards `rounded-2xl`, badges `rounded-full`.
- Cards `shadow-sm` → hover lift + larger shadow + brand border tint; primary/accent buttons
  carry a soft colored glow; hero bands are dark with radial brand glows + subtle grid overlay.

## Core components

Header / Nav / Footer · Buttons, badges, cards, section headers · Hero, TrustBar, ServiceGrid,
LocationGrid, RegionGrid · FAQAccordion, Testimonials, StatsStrip, ContactCTA · Breadcrumbs,
StickyCTA, BackToTop. Content markdown maps to these (H2 dividers, brand-dot bullets, numbered
circles, rounded tables with brand header row, brand-left-border blockquote callouts).

## Accessibility

Semantic landmarks, one h1, `lang`, `aria-label` on icon-only, skip-to-content, visible
`:focus-visible` (brand color on light, accent in dark footer), mobile menu animates + closes on
outside-click/Escape + manages focus, contrast ≥4.5:1. Keep a manual WCAG checklist.

## Images

Owned by the sibling skill **`seo-image-seo`** — self-hosting, formats (SVG/WebP),
width/height + alt + lazy-loading, standard sizes, OG card, and the freely-licensed
attribution practice. Components render per that spec; do not restate image rules here.
