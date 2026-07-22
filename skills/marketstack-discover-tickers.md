---
name: Discover tickers and exchanges
description: Look up which tickers, exchanges, indexes, and reference data the Marketstack API covers before requesting price data.
api: openapi/marketstack-v2-openapi.json
operations: [getTickerslist, getTickerinfo, getExchanges, getExchangesMicTickers]
---

# Discover tickers and exchanges

Reference-data endpoints, available on all plans.

1. Authenticate with `access_key=YOUR_ACCESS_KEY` on `https://api.marketstack.com/v2`.
2. List supported tickers with `getTickerslist` (`GET /tickerslist`); look up rich company/ticker detail with `getTickerinfo` (`GET /tickerinfo`) or `GET /tickers/{symbol}`.
3. List the 70+ supported exchanges with `getExchanges` (`GET /exchanges`); exchanges are identified by ISO 10383 MIC codes (e.g. `XNAS`, `XLON`). Drill into one with `GET /exchanges/{mic}` and enumerate its tickers with `getExchangesMicTickers` (`GET /exchanges/{mic}/tickers`).
4. Related reference surfaces: `GET /currencies`, `GET /timezones`, `GET /indexlist` + `GET /indexinfo` (indexes, Basic+), `GET /bondlist`/`GET /bond` and `GET /etflist`/`GET /etfholdings` (Basic+).
5. All list endpoints paginate with `limit` (max 1000) and `offset` inside the `{"pagination", "data"}` envelope; iterate until `offset + count >= total`.
6. Use the discovered `symbol` + `mic` pairs as inputs to the EOD and intraday skills.
