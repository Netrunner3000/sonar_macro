# Macro

_One of SONAR's tabs (`~/Documents/lab/active/sonar/sonar/macro/`). Its own git
repo, nested here — versioned separately from SONAR, but not a separate app.
Split out into its own project doc set on 2026-09-14 — see the parent SONAR
README.md for how the Macro tab fits into the rest of the app._

## What it does

The data layer behind SONAR's **Macro** tab. The short-horizon side of SONAR
prices a single hour and settles it against a real candle; long-horizon
analysis has no such anchor, so instead of a prediction it produces a
**regime**: what the rates, curve, volatility and labour picture actually say
right now, measured rather than narrated. The regime score is a transparent
heuristic — named components, published weights, no black box. It is a
description of conditions, not a forecast, and not advice.

## Data

Six FRED series, fetched keyless as plain CSV:

| Series | Meaning |
|---|---|
| `DGS10` | 10-year Treasury yield |
| `T10Y2Y` | 10-year minus 2-year spread — the curve. Negative = inverted, historically the most-watched recession signal there is. |
| `DFF` | Effective fed funds rate — the policy stance |
| `VIXCLS` | VIX — the market's own forward volatility estimate |
| `CPIAUCSL` | CPI index; year-over-year is computed here |
| `UNRATE` | Unemployment rate |

## Caching

These series update daily at most (`UNRATE`/`CPIAUCSL` monthly), so refetching
often buys nothing, and FRED rate-limits bursts. A six-hour disk cache (`_TTL`)
keeps fetches to roughly four per series per day and leaves the last good
values on disk if the network is down.

## Under the hood

| Location | Role |
|---|---|
| `series()` | Fetches (or reads cached) rows for one FRED series id. |
| `snapshot()` | Builds a `MacroSnapshot` from all six series. |
| `_classify()` | Scores the snapshot into the published, named regime components. |
| `MacroCache` | The `_TTL`-based disk cache wrapping `snapshot()`. |

## Requirements

Network access to FRED (keyless). No API key, no LLM involved.
