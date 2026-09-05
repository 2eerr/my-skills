---
name: water-damage-design-system
description: The premium UI/design system for Water Damage Pros US — brand voice, color tokens (emerald/amber/navy), typography (Plus Jakarta Sans + Inter), spacing & layout, radius/shadows, core components, accessibility rules, and the image attribution/licensing note. Use when building or styling site components and pages so content renders as a polished product page.
---

# Water Damage Design System

Single source of truth for look & feel. Content is rendered through these styles, so pages
must render as a premium product page, never a pasted document.

## Brand

- **Name:** Water Damage Pros US (configurable in `site.config.js`).
- **Personality:** premium, bold, modern, protective, trustworthy — national brand, local trust.
- **Copy tone:** helpful, confident, local, plain-English, short sentences, no fear-mongering.
- **Logo:** emerald shield mark + wordmark; renders `--color-primary-bright` on dark. Icons:
  `/favicon.svg`, `/apple-touch-icon.png` (180×180).

## Color tokens ("Premium bold")

| Token | Hex | Usage |
|---|---|---|
| `--color-primary` | `#0E7A3D` emerald | buttons, links, active, borders |
| `--color-primary-dark` | `#0A5C2E` | gradient stop, hover |
| `--color-primary-bright` | `#22A55C` | gradients, glows, dark-surface accents |
| `--color-primary-soft` | `#E8F5EE` | section tints, icon chips |
| `--color-accent` | `#F59E0B` amber | CTA, rating stars, urgency (sparingly) |
| `--color-accent-soft` | `#FEF3C7` | warning/offer badges |
| `--color-ink` | `#0F172A` slate-900 | primary text, headings |
| `--color-body` | `#4B5563` gray-600 | secondary text |
| `--color-muted` | `#9CA3AF` gray-400 | dark-band secondary text |
| `--color-line` | `#E5E7EB` | borders, dividers |
| `--color-surface` | `#FFFFFF` | cards, page bg |
| `--color-surface-soft` | `#F8FAFC` | alternating sections |
| `--color-navy` / `-2` / `--color-footer` | `#0B1220` / `#0E1628` / `#0B1220` | header, heroes, bands, footer |

**Contrast:** ink/body on white ≥4.5:1. Amber only for non-essential accents, never body text.
Text on amber = `--color-ink` (never white). Text on navy/footer = `--color-muted` or brighter
(never gray-500 or darker). Primary text on dark heroes is white.

## Typography

- **Headings:** Plus Jakarta Sans 600–800, `leading-[1.05]`, `tracking-tight`, sentence case.
- **Body:** Inter 400/500/600, `text-base`, `leading-relaxed`.
- **Scale (rem):** H1 3.5/4/hero 6 · H2 2.25/3 · H3 1.5 · H4 1.25 · body 1 · small .875 · micro .75.
- Fonts self-hosted (woff2), preloaded `fetchpriority="high"`, `ascent-override:80%` /
  `descent-override:20%` for zero CLS.

## Spacing, radius, shadows

- Base unit `0.25rem`; container `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8` (`.container-brand`).
- Section rhythm `py-16 lg:py-24`; 12-col grids; cards `grid-cols-1 sm:2 lg:3 xl:4`.
- Section heading rhythm: `.overline` → `.section-title` (H2) → `.section-sub`.
- Radius: buttons/inputs `rounded-xl`, cards `rounded-2xl`, badges `rounded-full`.
- Cards `shadow-sm` → hover `-translate-y-1` + `shadow-xl` + emerald border tint.
- Primary/accent buttons carry a soft colored glow; hero bands are dark navy with radial
  emerald glows + subtle grid overlay.

## Core components

Header / Nav / Footer · Buttons, badges, cards, section headers · Hero, TrustBar, ServiceGrid,
CityGrid, StateGrid · FAQAccordion, Testimonials, StatsStrip, ContactCTA · Breadcrumbs,
StickyCTA, BackToTop. Content markdown maps to these (H2 dividers, green-dot bullets, green
numbered circles, rounded tables with green header row, green-left-border blockquote callouts).

## Accessibility

Semantic landmarks, one h1, `lang="en"`, `aria-label` on icon-only, skip-to-content, visible
`:focus-visible` (green on light, amber in dark footer), mobile menu animates + closes on
outside-click/Escape + manages focus, contrast ≥4.5:1. Full 10-item WCAG checklist in
`docs/DESIGN.md` §11.1.

## Images (currently not rendered)

All `<img>` were removed site-wide; files remain in `public/images/` for later. When re-added:
every img needs `width`+`height`+descriptive `alt`, lazy-load below the fold, SVG for icons /
WebP for photos. Photos are self-hosted, sourced from Wikipedia/Wikimedia (PD / CC BY / CC BY-SA)
— keep the attribution table in `docs/IMAGES.md` accurate; re-fetch via
`python scripts/fetch-stock-images.py`. OG card + SVG illustrations are original.
