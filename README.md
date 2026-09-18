# waveStockAi

**Understand the market before you invest.**

waveStockAi is an Indian Stock Market Intelligence & Risk Platform demo. It combines stock discovery, technical and fundamental analysis, portfolio simulation, paper trading, risk analytics, screening, comparison, backtesting and a grounded AI explanation layer in one experience.

This workspace did not contain the referenced `Indian-Stock-Market-API` repository, so the deliverable is a self-contained demo foundation rather than a migration of existing endpoints. It is deliberately no-build: open `index.html` in a browser and the product runs immediately. The Worker API foundation is in `worker/index.js`.

## Run

Open `index.html` directly, or serve the folder with any static server:

```text
python -m http.server 4173
```

Then visit `http://localhost:4173`.

The UI uses browser local storage for the demo watchlist and paper portfolio. All visible market values are clearly marked delayed/demo; no live price is fabricated.

## Cloudflare Worker

`worker/index.js` is a dependency-free Worker entry point with a provider boundary, consistent responses, CORS handling, in-memory TTL caching, input validation, and deterministic technical/VaR calculations. It uses a primary → secondary → cached-last-good quote chain, so a provider outage never silently becomes a misleading live price.

Configure live feeds through Worker secrets or environment variables, never in the frontend:

```text
PRIMARY_QUOTE_ENDPOINT=https://your-primary-provider.example/quote
PRIMARY_QUOTE_TOKEN=...
SECONDARY_QUOTE_ENDPOINT=https://your-secondary-provider.example/quote
SECONDARY_QUOTE_TOKEN=...
```

Each endpoint must accept `?symbol=RELIANCE.NS` and return JSON containing a positive `price`; optional fields are `change`, `exchange`, and `as_of`. Use `GET /market/health` to monitor whether both feeds, one feed, or no feeds are configured. `GET /stock?symbol=RELIANCE.NS` always marks data as `live`, `stale`, or `unavailable`.

## Product surfaces

- Dashboard with portfolio value, risk score, VaR, watchlist, movers and sector pulse
- Search and stock detail pages with delayed-data labeling, chart, technical/fundamental/risk snapshot and “What if I buy?”
- Watchlist, portfolio, paper trading and deterministic trade-risk gate
- Risk Center with historical volatility, drawdown, VaR and CVaR context
- Compare, Screener, Backtesting, Alerts and grounded AI Copilot surfaces
- Responsive dark terminal UI with keyboard shortcut `Ctrl/Cmd + K`
- Reliability banner showing freshness, active fallback state, last feed check, and a manual feed-health check

## Disclaimer

waveStockAi is an educational and research platform. Market data and analytical outputs may be delayed or incomplete and are not financial advice. Verify information independently before making investment decisions.
