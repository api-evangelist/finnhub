---
name: Fetch OHLCV candles across stocks, forex, and crypto
description: Pull windowed candlestick (OHLCV) time series for a stock, forex pair, or crypto pair, resolving tradable symbols first.
api: openapi/finnhub-swagger-original.json
operations: [stock-candles, forex-exchanges, forex-symbols, forex-candles, crypto-exchanges, crypto-symbols, crypto-candles]
generated: '2026-07-22'
method: generated
---

# Fetch OHLCV candles across stocks, forex, and crypto

1. **Stocks** — `stock-candles` (`GET /stock/candle?symbol=<SYMBOL>&resolution=<1|5|15|30|60|D|W|M>&from=<unix>&to=<unix>`).
2. **Forex** — list venues with `forex-exchanges` (`GET /forex/exchange`), symbols with `forex-symbols` (`GET /forex/symbol?exchange=<venue>`), then `forex-candles` (`GET /forex/candle?symbol=<OANDA:EUR_USD>&resolution=...&from=...&to=...`).
3. **Crypto** — list venues with `crypto-exchanges` (`GET /crypto/exchange`), symbols with `crypto-symbols` (`GET /crypto/symbol?exchange=<venue>`), then `crypto-candles` (`GET /crypto/candle?symbol=<BINANCE:BTCUSDT>&resolution=...&from=...&to=...`).

## Rules

- Responses are column-oriented arrays (`o`,`h`,`l`,`c`,`v`,`t`) with an `s` status field — check `s == "ok"` before consuming; `no_data` is a valid empty result.
- Windows are `from`/`to` UNIX timestamps — page long histories by advancing the window, not by cursor (`conventions/finnhub-conventions.yml`).
- Respect the 30 calls/second cap when sweeping many symbols; on 429 back off (`errors/finnhub-problem-types.yml`).
- For live updates subscribe to trades over the WebSocket instead of polling candles (`asyncapi/finnhub-asyncapi.yml`).
