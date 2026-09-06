---
name: seo-data-packs
description: Build a per-location local data pack before writing local-SEO content (any niche) — verified sources (census, geography/climate agencies, city sites, community forums), the fill-in template (geography, climate, housing/property stock, neighborhoods, institutions, ordinances, seasonal calendar, local problems, expert observations), a worked example, and the pre-writing checklist. Use when gathering the real local facts that make a location's pages unique.
metadata:
  internal: true
---

# SEO Location Data Packs

A data pack is the collection of **real, verified local facts** about a location that drive
every money page. Without it you cannot write hyper-local, ≥90%-unique content. Build one per
location **before** writing that location's pages. (Writer rules: see the content-writing
skill.) Generalizes to any niche — swap the domain-specific "problems" for the service's own.

## Data sources (verify, never guess)

| Data type | Source |
|---|---|
| Population, county, coordinates | the project's location data (e.g. `src/data/cities.json`) |
| Housing/property stock (age, materials) | Census ACS, Zillow/realtor data, planning docs |
| Geography (water, terrain, exposure) | USGS, maps, Wikipedia |
| Nearby towns + metro | maps, Wikipedia |
| Landmarks / neighborhoods | maps, Yelp, city websites, neighborhood profiles |
| Climate zone + weather calendar | NOAA / met agencies, weather sites, local news |
| Local institutions / ordinances | city websites, permit offices, HOA rules, city code |
| **Local problems (real complaints)** | Quora, Reddit (`r/{city}` + niche subs), Facebook/Nextdoor groups, Google reviews of local providers, local news |

**Rules:** local details must be real and verified — no invented neighborhoods, landmarks, or
statistics; anything unverifiable is dropped or labeled an estimate. Mine real recurring
complaints and rewrite them as facts (never copy verbatim). Population is internal-only.

## Template (fill per location)

```markdown
# Data Pack: {Location}, {Region}
## Basic Facts
Location · Region · County · Population(~) · Coordinates · Metro area · Time zone
## Geography
Terrain · Elevation · Nearby water bodies · Nearby landmarks · Exposure (wind/tree/coast)
## Climate
Climate zone (Köppen) · Avg summer high · Avg winter low · Annual rainfall · Snow/ice ·
Key weather events (hail, hurricane, tornado, ice storm, first frost)
## Property / Housing Stock
Dominant build era/roof pitch · Foundation/type mix · Avg age · Key features (systems age, HVAC)
## Nearby Locations (2-3)
{name — distance — population}
## Neighborhoods/Districts (3-6 real)
{name — what it's known for (stock, age, geography) — why relevant to the service}
## Local Institutions
permit office · HOA rules · major employers · university · adjuster practices
## Local Ordinances/Quirks
permit requirements · licensing/storm-chaser rules · material restrictions
## Seasonal Calendar
| Month | Concerns for this service | Why |  (all 12 months)
## Top Service-Relevant Problems (ranked 1-5)
## Local Problems (2-3 real complaints from forums/reviews/news)
## Local Expert Observations (3-5)
neighborhoods & build age · stock & problems · weather timing · institutions · geography
```

## Worked example (condensed, generic) — a mid-size US prairie town

- County + metro named; humid subtropical; ~50" rain; spring straight-line wind + late-spring
  hail; occasional ice storms.
- Stock: ~85% slab, 1990s–2010s builds.
- Neighborhoods: newer wind-exposed subdivisions on open ground; older downtown with mature
  trees and shaded north slopes.
- Institutions: county building dept (permits); a large base/employer drives rentals; school
  district largest employer.
- Top problems: winter pipe bursts in older subdivisions; spring storm/hail claims; aging
  1990s systems; humidity-driven growth on shaded slopes; ice dams on attic-heated homes.
- Expert notes: unobstructed wind → recurring storm calls; claim volume spikes after spring
  storms; adjuster inspections weekly.

## Regional notes pattern

For each region, capture 4–6 recurring, area-specific drivers (climate, geography, housing
era, regulations) that shape which problems and subservices matter there — e.g. desert monsoon
+ UV, coastal storm-surge + salt, northern freeze/thaw + ice dams, humid-southeast mold.

## Pre-writing checklist (confirm before any money page)

- [ ] location/region/county/population · [ ] geography · [ ] climate zone + weather calendar
- [ ] property/housing stock · [ ] 2–3 nearby towns + metro · [ ] 1–2 real neighborhoods
- [ ] local institutions · [ ] ordinances/quirks · [ ] 12-month seasonal calendar
- [ ] top 5 problems ranked · [ ] 2–3 real local problems · [ ] 3–5 expert observations
