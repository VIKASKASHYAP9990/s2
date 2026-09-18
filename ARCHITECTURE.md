# Architecture

The intended production shape is:

```text
React/Vite frontend
        ↓
waveStockAi REST API (Cloudflare Worker)
        ↓
MarketDataProvider
   ├── Yahoo Finance adapter
   ├── NSE/BSE search adapter
   └── KV/cache + stale-while-revalidate
```

The current frontend is a no-build prototype so a judge can run it immediately. Its state boundaries mirror the production design: provider data is not requested directly by UI components, deterministic analytics are separate from explanation copy, and every market surface exposes provider/data status.

Production hardening should add Cloudflare KV or Durable Objects for cache/request coalescing, a real symbol universe, authenticated per-user persistence, strict origin allow-lists, provider timeouts/backoff, and a real AI provider behind `/ai/analyze`.
