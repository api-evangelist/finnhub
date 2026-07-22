---
name: Research a public company
description: Build a company research brief — profile, headline fundamentals, peers, analyst recommendation trends, earnings history, and recent news for one symbol.
api: openapi/finnhub-swagger-original.json
operations: [company-profile2, company-basic-financials, company-peers, recommendation-trends, company-earnings, company-news]
generated: '2026-07-22'
method: generated
---

# Research a public company

1. **Profile** — `company-profile2` (`GET /stock/profile2?symbol=<SYMBOL>`): name, exchange, industry, market cap, IPO date, weburl. Also accepts `isin` or `cusip`.
2. **Fundamentals** — `company-basic-financials` (`GET /stock/metric?symbol=<SYMBOL>&metric=all`): P/E, EPS, margins, 52-week high/low.
3. **Peers** — `company-peers` (`GET /stock/peers?symbol=<SYMBOL>`) for sector comparables.
4. **Analyst view** — `recommendation-trends` (`GET /stock/recommendation?symbol=<SYMBOL>`): strong-buy/buy/hold/sell/strong-sell counts per month.
5. **Earnings track record** — `company-earnings` (`GET /stock/earnings?symbol=<SYMBOL>`): actual vs estimate surprises for past quarters.
6. **Recent news** — `company-news` (`GET /company-news?symbol=<SYMBOL>&from=<YYYY-MM-DD>&to=<YYYY-MM-DD>`); North American companies only.

## Rules

- Every call needs the API key (`token` query param or `X-Finnhub-Token` header).
- Spread calls to stay under the plan limit and the 30 calls/second cap; on 429 back off and retry (`conventions/finnhub-conventions.yml`).
- Some datasets (estimates, deeper fundamentals) are premium-plan gated — treat empty payloads on the free tier as an access limitation, not an absence of data.
