---
title: "Privacy Mode: Server-Authorizes, Client-Executes Pattern"
date: 2026-04-07
category: "architecture"
source: "kodi"
---

When implementing privacy features that mask sensitive data (like hiding transaction amounts), the "server-authorizes, client-executes" approach provides the best UX-security balance.

## The Problem

Pure server-side masking prevents users from revealing values locally. Pure client-side masking relies on the client to respect privacy settings.

## The Solution

Server returns both the value and a mask flag:

```json
{
  "amount": 12345.67,
  "mask": true
}
```

The client:
1. Masks by default when `mask: true`
2. Reveals locally when user clicks (no extra request)
3. Server controls *what* gets masked via the flag

## Why This Works

- **Smooth UX**: No loading delays when revealing values
- **Server control**: Backend decides which data is sensitive
- **Flexible**: Easy to toggle per-user, per-field, or per-context
- **Secure enough**: Data is transmitted but not displayed by default

## Trade-offs

| Approach | Pros | Cons |
|----------|------|------|
| Server-authorizes, client-executes | Fast reveal, server control | Data still transmitted |
| Pure server masking | Most secure | Slow reveal (extra request), can't pre-load |
| Pure client masking | Fastest | Client can bypass, relies on frontend |

## When to Use

- Banking/finance apps (account balances)
- Healthcare apps (sensitive metrics)
- Any feature needing "peek" functionality

## Reference

Implemented in [qepo17/koin#115](https://github.com/qepo17/koin/issues/115)
