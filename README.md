# Flights Project: Seasonal Destination Mix in U.S. Domestic Air Travel

Do U.S. city markets hold different shares of domestic arriving passengers during
winter break than during summer break — and which cities shift the most?

Business Analytics II · University of Pittsburgh · Fall 2026

---

## The question

Ranking destination cities by raw passenger volume returns the same answer in both
seasons: Atlanta, Chicago, New York, Los Angeles, Dallas. Those are connecting hubs,
so that ranking measures network structure, not where people choose to go.

This project compares each city market's **share** of national domestic arriving
passengers between the two periods, producing a seasonality index
(winter share ÷ summer share). Above 1 is winter-skewed, below 1 is summer-skewed.
Shares rather than totals, because winter break spans two months and summer spans
three — raw totals are not comparable.

## Data

| | |
|---|---|
| Source | U.S. DOT / Bureau of Transportation Statistics — T-100 Domestic Market |
| Portal | https://transtats.bts.gov/ |
| Input grain | One carrier's traffic on one origin–destination market, one month, one service class |
| Periods | Winter: Dec 2024, Jan 2025 · Summer: Jun–Aug 2025 |
| License | U.S. Government Work — public domain in the U.S. (17 U.S.C. §105) |
| Retrieved | `[date]` |

Download parameters are in [`SOURCE.md`](SOURCE.md). The raw extract is committed here
because the license permits redistribution, and because federal datasets have been
removed and altered during 2025–2026 — the archived copy is what this analysis runs
against.

## Repo structure

```
.
├── README.md                              this file
├── SETUP.md                               Git workflow and troubleshooting
├── SOURCE.md                              where the data came from, and exactly how
├── proposal.md                            Session 1 deliverable
├── 01_explore_clean.ipynb                 Session 2 deliverable
└── data/
    ├── t100_market_*.csv                  monthly extracts, as downloaded
    ├── L_CITY_MARKET_ID.csv               city-market lookup
    └── processed/
        └── destination_month_passengers.csv   the one clean dataset
```

## Running it

```bash
pip install pandas jupyter
```

Open `01_explore_clean.ipynb` and click **Restart & Run All**. The raw files are read
with `dtype=str` on purpose, so the type errors are visible rather than silently
guessed away.

The notebook works through the six failure modes in order, with a check printed before
each fix and an assertion after it. It writes one clean dataset: one row per
destination city market per month.

## What the cleaning found

See the final markdown cell of the notebook. The failure that would have gone
unnoticed was **join fanout** on the city-market lookup table. A duplicated code there
grew the row count on a plain left join — nothing errored, and passenger totals rose in
a way that looked like real traffic. The merge is now guarded with
`validate='many_to_one'` and a row count printed on both sides.

## Known limitations

1. T-100 is monthly, so December–January is a proxy for winter break and cannot
   isolate the actual break weeks.
2. T-100 Market records each carrier's ticketed segment, so connecting passengers can
   appear in two markets. Metro-level aggregation reduces but does not eliminate this.
3. Arrivals include returning residents; the data cannot separate visitors from
   locals.
