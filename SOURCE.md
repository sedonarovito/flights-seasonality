# Data Provenance

## Dataset

**T-100 Domestic Market**
U.S. Department of Transportation, Bureau of Transportation Statistics,
Office of Airline Information. Filed monthly by U.S. carriers under 14 CFR Part 241.

- **Portal:** https://transtats.bts.gov/
- **Aviation database list:** https://transtats.bts.gov/databases.asp?Z1qr_VQ=E&Z1qr_Qr5p=N8vn6v10&f7owrp6_VQF=D
- **Navigation:** Data Finder → Aviation → *Air Carrier Statistics (Form 41 Traffic)*
  → *T-100 Domestic Market* → Download
- **Retrieved:** `YYYY-MM-DD` ← fill in the day you download
- **Retrieved by:** Sedona Rovito

TranStats stores the selected table in a session variable, so there is no permanent
direct file URL to record. The exact form selections below are the reproducibility
substitute. Verified live as of September 2026; data available through May 2026.

## Download parameters

Run the download form five separate times:

| Run | Year | Month | Save as |
|---|---|---|---|
| 1 | 2024 | December | `data/t100_market_2024_12.csv` |
| 2 | 2025 | January | `data/t100_market_2025_01.csv` |
| 3 | 2025 | June | `data/t100_market_2025_06.csv` |
| 4 | 2025 | July | `data/t100_market_2025_07.csv` |
| 5 | 2025 | August | `data/t100_market_2025_08.csv` |

**Fields to select** — leave everything else unchecked to keep the files small:

- Summaries → `Passengers`, `Distance`
- Time Period → `Year`, `Month`
- Carrier → `UniqueCarrier`, `AirlineID`
- Origin → `Origin`, `OriginCityMarketID`, `OriginCityName`
- Destination → `Dest`, `DestCityMarketID`, `DestCityName`
- Other → `Class`, `DataSource`

Downloads arrive zipped. The CSV sometimes has a trailing comma that pandas reads as
an unnamed empty column — the notebook drops it.

## Lookup table

`L_CITY_MARKET_ID` maps `DEST_CITY_MARKET_ID` to metro names such as
"New York City, NY (Metropolitan Area)". Save as `data/L_CITY_MARKET_ID.csv`.

https://transtats.bts.gov/Download_Lookup.asp?Y11x72=Y_PVgl_ZNeXRg_VQ

Other lookups, if needed:

| Lookup | Link |
|---|---|
| Unique Carrier → airline name | https://transtats.bts.gov/Download_Lookup.asp?Y11x72=Y_haVdhR_PNeeVRef |
| Service Class codes | https://transtats.bts.gov/Download_Lookup.asp?Y11x72=Y_fReiVPR_PYNff |

**Note:** the lookup table has been observed to contain duplicated codes. The
notebook dedupes it before merging and asserts the row count is unchanged — see the
join-fanout section.

## License

Work of the U.S. federal government. Not subject to copyright protection in the
United States under 17 U.S.C. §105; listed on data.gov as a U.S. Government Work.
Redistribution is permitted, so the raw extract is committed to this repository.

Confirm the license wording on the dataset page on the day of retrieval.

## Changes observed after retrieval

*(Record anything here if the source is later reorganized, altered, or removed.)*

- `YYYY-MM-DD` — none observed
