# Project Proposal — Seasonal Destination Mix in U.S. Domestic Air Travel

**Sedona Rovito** · Business Analytics II · Fall 2026

---

## 1. The question

**Do U.S. city markets hold different shares of domestic arriving passengers during
winter break than during summer break — and which cities shift the most?**

Split: destination city market. Measure: share of national domestic arriving
passengers in the period. Comparison: winter break months vs. summer break months.

I am deliberately not asking "what are the top destination cities," because the raw
top ten will be Atlanta, Chicago, New York, Los Angeles and Dallas in both seasons.
Those are connecting hubs, and that ranking is a fact rather than a finding.
Converting to share-of-period makes large and small cities comparable and isolates
the seasonal *mix* rather than the seasonal *volume*.

## 2. The dataset

| | |
|---|---|
| **Source** | U.S. DOT, Bureau of Transportation Statistics — Air Carrier Statistics (Form 41 Traffic), **T-100 Domestic Market** |
| **Portal** | https://transtats.bts.gov/ |
| **License** | U.S. Government Work — public domain in the U.S. (17 U.S.C. §105). Redistributable; the raw extract is committed to the repo. |
| **Retrieved** | `[date you download it]` |
| **File size** | `[fill in]` MB (selected fields only, five months) |
| **Periods** | Winter: Dec 2024, Jan 2025 · Summer: Jun, Jul, Aug 2025 |
| **Supporting file** | `L_CITY_MARKET_ID` lookup, same site, for metro names |

Pulling only five months and only the needed fields keeps the extract well under
GitHub's 50 MB comfort limit. A full-year pull is unnecessary.

## 3. The grain

**One row is one carrier's reported traffic on one origin–destination market, in one
month, for one service class.** Not one flight, and not one passenger.

Unique key: `YEAR + MONTH + UNIQUE_CARRIER + ORIGIN + DEST + CLASS`

| Check | Value |
|---|---|
| `len(df)` | `[fill from notebook output]` |
| unique on grain key | `[fill from notebook output]` |

`01_explore_clean.ipynb` prints both numbers and asserts they match after deduplication.
Expected magnitude is roughly 150k–300k rows across five months, inside the
assignment's sweet spot.

## 4. The comparison

- **Split by:** `DEST_CITY_MARKET_ID` (metro level, so JFK/LGA/EWR count as one New
  York and MCO/SFB as one Orlando)
- **Measure:** `PASSENGERS`, converted to each city's percent of all domestic
  arriving passengers in that period
- **Statistic:** seasonality index per city = winter share ÷ summer share. Above 1 is
  winter-skewed, below 1 summer-skewed.
- **Filters:** `CLASS == 'F'` (scheduled passenger service) and `PASSENGERS > 0`, to
  drop charter and cargo-only records. U.S. territories kept in — San Juan and the
  USVI are U.S. destinations and are likely to matter here.

## 5. Why either answer is interesting

**If the shares differ:** leisure demand relocates by season rather than merely rising
and falling — sun and ski metros absorb a disproportionate slice in December and
January, while summer spreads toward coastal, mountain and long-haul-gateway cities.
That is a direct input to airline seasonal capacity planning, hotel and resort revenue
management, and seasonal hiring. It also means a hospitality business's comparable
figures are only interpretable against the same season, not the prior quarter.

**If the shares are flat:** seasonality in U.S. air travel is a volume story and not a
mix story — everyone flies more in July, but to roughly the same places in roughly the
same proportions. That would suggest the route network rather than destination
preference sets where people go, and would undercut the common assumption that winter
travel is distinctively a warm-weather-getaway market.

## 6. Known limitations, stated up front

1. **Months are a proxy for breaks.** December contains three non-break weeks. T-100
   is monthly, so I cannot isolate Dec 18 – Jan 5. This is a measurement limitation,
   not something to paper over.
2. **Connections inflate hubs.** T-100 Market records each carrier's ticketed segment,
   so a passenger connecting through Charlotte can appear in two markets. Metro-level
   aggregation reduces this but does not remove it. DB1B would fix it at the cost of
   quarterly-only timing.
3. **Arrivals are not visitors.** Passengers landing in Orlando in July include
   residents coming home.
4. **Unequal window lengths** are handled by using shares within each period. A raw
   winter-versus-summer total would be meaningless at two months against three.

---

*Archiving note:* per the Session 1 guide, the extract is downloaded and committed the
day it is retrieved, with `SOURCE.md` recording the exact query parameters. Federal
datasets have been removed and altered during 2025–2026, and this is a federal source.
