---
name: Get real-time and intraday prices
description: Retrieve real-time stock prices and intraday bars from the Marketstack API, respecting plan gating (intraday requires Basic+, real-time requires Professional+).
api: openapi/marketstack-v2-openapi.json
operations: [getStockprice, getIntraday, getIntradayLatest]
---

# Get real-time and intraday prices

Plan-gated: intraday data (IEX, US) requires Basic or higher; real-time stock prices and commodities require Professional or higher. A `403 function_access_restricted` error means the account's plan does not include the endpoint - surface an upgrade message instead of retrying.

1. Authenticate with `access_key=YOUR_ACCESS_KEY` on `https://api.marketstack.com/v2`.
2. For current quotes, call `getStockprice` (`GET /stockprice`) with `symbols=AAPL` (Professional+).
3. For intraday bars, call `getIntraday` (`GET /intraday`) with `symbols`, optional `date_from`/`date_to`; `getIntradayLatest` (`GET /intraday/latest`) returns the most recent intraday bar per symbol. Per-ticker equivalents exist at `GET /tickers/{symbol}/intraday`.
4. Paginate with `limit`/`offset` and read the `{"pagination", "data"}` envelope, same as EOD.
5. Throttle to at most 5 requests per second (`429 rate_limit_reached`); budget the monthly plan quota (`429 too_many_requests`).
