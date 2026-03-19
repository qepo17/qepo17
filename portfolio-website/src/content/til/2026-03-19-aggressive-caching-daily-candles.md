---
title: "Cache Daily Stock Candles Aggressively — They Barely Change"
date: 2026-03-19
category: dev
source: kodi
---

When building a stock technical analysis dashboard, I initially cached daily candle data for just 5 minutes. That's way too conservative — daily candles only change once per day after market close.

## The Insight

Daily OHLCV (Open/High/Low/Close/Volume) candle data is essentially immutable for historical days. Even the current day's candle only updates during market hours. For IDX (Indonesia Stock Exchange), that's 09:00–15:30 WIB.

## Two-Layer Caching Strategy

**Layer 1: Raw data cache (1 hour TTL)**
Cache the fetched daily history from the data provider (Yahoo Finance). Even during market hours, 1-hour staleness is fine for daily timeframe analysis.

**Layer 2: Computed TA summary cache (30 min TTL, per user)**
Technical analysis computations (RSI, MACD, Bollinger, confluence scores) are expensive when run across an entire watchlist. Cache the computed results per user and invalidate on watchlist changes (add/remove stock).

```typescript
// Invalidate TA cache when watchlist changes
export function invalidateTACache(userId: string) {
  taSummaryCache.delete(userId);
}
```

**The takeaway:** Match your cache TTL to how often the underlying data actually changes, not to how often users might request it. For daily candles: hours, not minutes.
