---
name: water-damage-locations
description: Geographic coverage model for Water Damage Pros US — 50 states + DC, the 1,000+ population city filter (11,447 cities), the four regions, the location data files (states.json, cities.json, census CSVs), and the content-gated publishing rules (whole-state vs per-city vs per-hub). Use when generating or validating state/city location pages.
---

# Water Damage Locations Model

Single source of truth for "where do we cover."

## Scope

| Metric | Count |
|---|---|
| States | 50 (+ DC = 51 entries in `states.json`) |
| Qualifying cities | 11,447 |
| Services per city | 39 |
| Money pages | 11,447 × 39 = 446,433 |
| Total site pages | 9 core + 39 service + 51 state + 11,447 city + 446,433 money = **457,980** |

DC is in `states.json` but has **0 qualifying cities** → produces a state page only.

## Population filter

Only cities with **population ≥ 1,000** qualify (enforced by `scripts/validate-data.js`).
This is the rank-and-rent sweet spot — enough local search volume to win with a localized
page. Source: US Census ACS (2020 decennial + ACS 5-year). **Population is internal-only** —
used for the filter and as a writer reference; never featured on published pages.

## Data files

| File | Contents |
|---|---|
| `src/data/states.json` | 51 entries — name, abbrev, slug, region, cities count |
| `src/data/cities.json` | 11,447 entries — name, state, abbrev, slug, county, population, lat/long |
| `src/data/census-population-estimates.csv` | raw Census estimates (master 1K+ filter source) |
| `src/data/state-city-lists-source.csv` | per-state top-city seed lists |
| `scripts/gen-locations.js` | regenerates the JSON from `docs/LOCATIONS.md` Appendix A |

City record: `{ name, state, abbrev, slug, county, population, latitude, longitude }`.
State record: `{ name, abbrev, slug, region, cities }`.

## Regions

States group into **4 regions** (used by the Locations mega menu and state-page organization).
Full per-state tables and the complete city inventory (Appendix A) live in `docs/LOCATIONS.md`
— read that file for a specific state's city list and populations.

## Content-gated publishing rules

- **Whole state publishes only when 100% complete** — state page + every city page + every
  city×service money page carry `unique: true` (`isCompleteState` in `src/lib/data.js`).
- **Cities publish progressively, per city** — a city renders once its own city page + all 39
  money pages are `unique: true` (`isCompleteCity`); it does not wait for the rest of the state.
- **Hubs publish per service** — `/services/{slug}/` renders once `hubs/{slug}.md` is `unique: true`.
- **Header Locations mega menu** is hidden until `completeStates()` returns ≥1 state; it lists
  only fully-published states, grouped by region, and updates automatically on next build.
- The same `completeStates()` powers the Footer (top 8 states), `/locations/`, and the homepage.

## State page order

Write all of a state's city pages (and their money pages) first; write the state page last
(see the content-autopilot skill). A state's tracker row re-derives to `[x]` only when the
state page + every city + every money page is unique.
