---
name: bettip-draw-archive-analysis
description: Pull the full historical draw archives (JSON or CSV) from the BetTip Draws API for frequency, overdue-number, or trend analysis — with the ball-pool caveats that make naive counts wrong.
api: BetTip Draws API
base: https://bettip.co.za/api/v1
operations:
  - GET /uk49s/draws.json
  - GET /uk49s/draws.csv
  - GET /uk49s/{year}.json
  - GET /uk49s/draws/{date}.json
  - GET /lotto/draws.json
  - GET /lotto/draws.csv
  - GET /lotto/payouts.json
  - GET /gosloto/draws.json
  - GET /gosloto/draws.csv
  - GET /world/draws.json
generated: '2026-09-09'
method: generated
source: openapi/bettip-draws-api-openapi.json + llms/bettip-draws-api-llms.txt
note: Operations are cited as METHOD + path because the provider's spec declares no operationIds.
---

# Analyze historical draw archives

1. Fetch the archive you need — whole archives are one file, no pagination:
   - UK49s (5,800+ draws since 2018-09-01): `GET /uk49s/draws.json` or `/uk49s/draws.csv`; a single year via `GET /uk49s/{year}.json` (2018 onward); a single date via `GET /uk49s/draws/{date}.json` (YYYY-MM-DD; 404 when no draw exists).
   - SA National Lottery (7,400+ draws since 2015-01-02): `GET /lotto/draws.json` or `.csv`; payouts per division via `GET /lotto/payouts.json`.
   - Gosloto and world draws: `GET /gosloto/draws.json`, `GET /world/draws.json` — shallow archives collected forward from 2026-08-29; do not compute frequency tables from them.
2. CSV endpoints return `text/csv` for spreadsheet use; JSON archives are arrays of draw objects (`date`, `draw`, `numbers`, `booster`, `official`).
3. Count against eligible draws, not all draws: the SA Lotto / Lotto Plus 1 ball pool widened from 1-52 to 1-58 on 2025-09-24, so balls 53-58 were drawable in only a fraction of the archive. An all-time count that ignores this misreads those balls as cold.
4. Archives are archival: stored draws are never rewritten, and SA Lottery draws are dual-sourced (written only when the settlement feed and an independent archive agree).
5. Cache the big archive files and revalidate with the ETag; they change at most a few times per day.
6. Attribute BetTip (link to https://bettip.co.za/, CC BY 4.0), and never present the archive as a basis for predicting future draws.
