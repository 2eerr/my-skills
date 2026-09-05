---
name: water-damage-data-packs
description: Build a per-city local data pack before writing Water Damage Pros US content — verified sources (Census, USGS, NOAA, city sites, Quora/Reddit/Nextdoor/news), the fill-in template (geography, climate, housing stock, neighborhoods, institutions, ordinances, seasonal calendar, local problems, expert observations), a worked example, and the pre-writing checklist. Use when gathering the real local facts that make a city's pages unique.
---

# Water Damage City Data Packs

A data pack is the collection of **real, verified local facts** about a city that drive every
money page. Without it you cannot write hyper-local, ≥90%-unique content. Build one per city
**before** writing that city's pages. (Writer rules themselves: see the content-writing skill.)

## Data sources (verify, never guess)

| Data type | Source |
|---|---|
| Population, county, coordinates | `src/data/cities.json` (already in project) |
| Housing stock (age, material mix) | Census ACS, Zillow, city planning docs |
| Geography (rivers, lakes, terrain, exposure) | USGS, Google Maps, Wikipedia |
| Nearby towns + metro | Google Maps, Wikipedia |
| Landmarks / neighborhoods | Google Maps, Yelp, city websites, Zillow |
| Climate zone + weather calendar | NOAA, Weather.com, local news |
| Local institutions / ordinances | city websites, permit offices, HOA rules, city code |
| **Local water-damage problems (real complaints)** | Quora, Reddit (`r/{city}`, `r/WaterDamage`), Facebook/Nextdoor groups, Google reviews of local restoration cos., local news storm coverage |

**Rules:** local details must be real and verified — no invented neighborhoods, landmarks, or
statistics; anything unverifiable is dropped or labeled an estimate. Mine real recurring
complaints and rewrite them as facts (never copy verbatim). Population is internal-only.

## Template (fill per city)

```markdown
# Data Pack: {City}, {State}
## Basic Facts
City · State · County · Population(~) · Coordinates · Metro area · Time zone
## Geography
Terrain · Elevation · Nearby water bodies · Nearby landmarks · Exposure (wind/tree/coast)
## Climate
Climate zone (Köppen) · Avg summer high · Avg winter low · Annual rainfall · Snow/ice ·
Key weather events (hail, hurricane, tornado, ice storm, first frost)
## Housing Stock
Dominant home age/roof pitch · Foundation mix (slab/crawlspace/basement) · Avg home age ·
Key features (plumbing age, HVAC, attic vs vaulted)
## Nearby Cities (2-3)
{name — distance — population}
## Neighborhoods/Districts (3-6 real)
{name — what it's known for (housing, age, geography) — why relevant to water damage}
## Local Institutions
permit office · HOA material rules · major employers · university · adjuster practices
## Local Ordinances/Quirks
restoration permit requirements · storm-chaser/licensing rules · HOA restrictions
## Seasonal Water Damage Calendar
| Month | Concerns | Why |  (all 12 months)
## Top Water Damage Concerns (ranked 1-5)
## Local Water Damage Problems (2-3 real complaints from Quora/Reddit/FB/Nextdoor/news)
## Local Expert Observations (3-5)
neighborhoods & home age · housing stock & problems · weather timing · institutions · geography
```

## Worked example (condensed) — Cabot, Arkansas

- County Lonoke; metro Little Rock–NLR–Conway; humid subtropical (Cfa); ~50" rain; spring
  straight-line wind + May–June hail; occasional ice storms.
- Housing: ~85% slab, 1990s–2010s builds, vaulted great rooms.
- Neighborhoods: East Cabot (HWY 89 subdivisions, wind-exposed open prairie); Downtown (older
  1970s–80s homes, mature trees, north-slope moss).
- Institutions: Lonoke County Building Dept (restoration permits); Little Rock AFB drives
  rentals; Cabot School District largest employer.
- Top concerns: winter pipe bursts in older subdivisions; spring hail claims; aging 1990s water
  systems; humid-summer moss/algae on shaded slopes; ice dams on attic-heated homes.
- Expert notes: nothing breaks prairie wind east of HWY 89 → recurring wind calls; claim volume
  spikes May–June; adjuster inspections weekly.

## State-specific notes (examples)

- **AZ** desert: extreme heat, July–Aug monsoon + haboobs, UV pipe degradation, flat/low-slope,
  HOA material covenants.
- **AL** humid subtropical: Gulf wind/rain, high humidity → mold, slab dominant, coastal flood.
- **AK** subarctic: ice dams + snow loading dominate, short build season, steep roofs, critical
  attic ventilation.
- **AR** humid subtropical: spring straight-line wind is the top cause, hail in Little
  Rock/Jonesboro corridors, Ozark vs Delta differences, Grand Prairie wind exposure.

## Pre-writing checklist (confirm before any money page)

- [ ] city/state/county/population · [ ] geography · [ ] climate zone + weather calendar
- [ ] housing stock · [ ] 2–3 nearby towns + metro · [ ] 1–2 real neighborhoods
- [ ] local institutions · [ ] ordinances/quirks · [ ] 12-month seasonal calendar
- [ ] top 5 concerns ranked · [ ] 2–3 real local problems · [ ] 3–5 expert observations
