---
name: Fetch end-of-day price history for a ticker
description: Pull historical end-of-day OHLCV bars for one or more stock tickers from the Marketstack API, handling auth, pagination, and plan-gated errors.
api: openapi/marketstack-v2-openapi.json
operations: [getEod, getTickersSymbolEod, getEodLatest]
---

# Fetch end-of-day price history for a ticker

Available on ALL plans, including Free (1 year of history; 10 years on Basic, 15+ on Professional/Business).

1. Authenticate every request by appending `access_key=YOUR_ACCESS_KEY` as a query parameter (no headers, no OAuth). Base URL: `https://api.marketstack.com/v2`.
2. For one or more symbols, call `getEod` (`GET /eod`) with `symbols=AAPL,MSFT` (comma-separated, max 100 per request). Filter with `date_from`/`date_to` (YYYY-MM-DD) and `sort` (DESC default).
3. For a single ticker, `getTickersSymbolEod` (`GET /tickers/{symbol}/eod`) is equivalent; `getEodLatest` (`GET /eod/latest`) returns only the most recent bar per symbol.
4. Paginate with `limit` (default 100, max 1000) and `offset`; the response envelope is `{"pagination": {limit, offset, count, total}, "data": [...]}`. Loop while `offset + count < total`.
5. Each bar carries `open/high/low/close/volume`, adjusted variants (`adj_*`), `split_factor`, `dividend`, `symbol`, `exchange` (ISO 10383 MIC), and `date`.
6. Errors arrive as `{"error": {"code", "message"}}`: `401 invalid_access_key` (bad key), `422 validation_error` (e.g. more than 100 symbols), `429 rate_limit_reached` (stay under 5 req/sec - throttle and retry with backoff), `429 too_many_requests` (monthly quota exhausted - do not retry).
