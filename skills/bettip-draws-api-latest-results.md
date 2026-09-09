---
name: bettip-latest-lottery-results
description: Fetch the latest UK49s, SA National Lottery, Gosloto or world draw results from the free, keyless BetTip Draws API, with correct citation and labelling.
api: BetTip Draws API
base: https://bettip.co.za/api/v1
operations:
  - GET /uk49s/latest.json
  - GET /lotto/latest.json
  - GET /gosloto/latest.json
  - GET /world/latest.json
  - GET /uk49s/next.json
  - GET /gosloto/next.json
  - GET /lotto/next.json
  - GET /world/next.json
generated: '2026-09-09'
method: generated
source: openapi/bettip-draws-api-openapi.json + llms/bettip-draws-api-llms.txt
note: Operations are cited as METHOD + path because the provider's spec declares no operationIds.
---

# Get the latest lottery results

No key, no account, CORS-open. Every operation is a plain GET.

1. Pick the family endpoint:
   - UK49s: `GET https://bettip.co.za/api/v1/uk49s/latest.json` — `byDraw` holds the latest result per draw name; `recent` holds recent draws.
   - SA National Lottery: `GET https://bettip.co.za/api/v1/lotto/latest.json` (SA Lotto, Lotto Plus 1, PowerBall, PowerBall Plus, Daily Lotto).
   - Russia Gosloto: `GET https://bettip.co.za/api/v1/gosloto/latest.json`.
   - World draws: `GET https://bettip.co.za/api/v1/world/latest.json`.
2. For the next draw and countdown, use the family's `next.json`. Gosloto/lotto/world `next.json` accept an optional `?game=` query; an unknown game returns 404 ("Unknown game") — omit the parameter to get every game.
3. Times are given in UTC (`atUtc`) and South African time (`atSast`); Gosloto's timetable is published in SAST, not Moscow time. Some schedule entries are estimates — check `estimated` and `scheduleStatus` before presenting a time as official.
4. Label draws correctly: for UK49s, only Lunchtime and Teatime are official 49s draws; Brunchtime and Drivetime are operator draws (`official: false` in the data).
5. Cache responses and revalidate (responses carry `Cache-Control: public, max-age=300` and an ETag). Never predict future draws from results — every draw is independent.
6. Attribute the data: credit BetTip with a link to https://bettip.co.za/ wherever the data is shown (CC BY 4.0).
