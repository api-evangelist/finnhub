---
name: Get a real-time stock quote
description: Resolve a company name to a ticker and fetch its real-time quote, checking market status so a prior close is never presented as a live price.
api: openapi/finnhub-swagger-original.json
operations: [symbol-search, market-status, quote]
generated: '2026-07-22'
method: generated
---

# Get a real-time stock quote

1. **Authenticate every call** with the API key as `token=<key>` in the query string or the `X-Finnhub-Token: <key>` header (see `authentication/finnhub-authentication.yml`). Keys come from https://finnhub.io/register.
2. **Resolve the symbol** — if you have a company name, call `symbol-search` (`GET /search?q=<name>`) and pick the best match (US common stock first on the free tier).
3. **Check market state** — call `market-status` (`GET /stock/market-status?exchange=US`) so you can label the price as live or prior close.
4. **Fetch the quote** — call `quote` (`GET /quote?symbol=<SYMBOL>`): `c` current, `o` open, `h` high, `l` low, `pc` previous close, `t` timestamp.

## Rules

- All operations are GET and read-only — safe to retry (see `conventions/finnhub-conventions.yml`).
- On HTTP 429 back off: per-plan limits plus a hard 30 calls/second cap (`errors/finnhub-problem-types.yml`).
- 401 means a missing/invalid token — do not retry without fixing the key.
- For continuous prices prefer the WebSocket stream (`asyncapi/finnhub-asyncapi.yml`) — one connection per API key.
