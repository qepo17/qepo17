---
title: "API Pagination: Capping Limits to Prevent DoS"
date: "2026-05-05"
category: "Backend"
source: "kodi"
---

# API Pagination: Capping Limits to Prevent DoS

Today I fixed an unbounded query vulnerability where `GET /transactions` had no `LIMIT` clause, allowing a single request to fetch the entire database. On a table with millions of rows, this is a trivial DoS vector.

## The Problem

The endpoint accepted `limit` and `offset` query params but:
- Had **no default limit** — omitting `limit` returned every row
- Had **no maximum cap** — a client could request `?limit=99999999`
- Returned **no pagination metadata** — clients couldn't tell if there was more data

## The Fix

```typescript
const DEFAULT_LIMIT = 100;
const MAX_LIMIT = 500;

const limit = Math.min(
  parseInt(req.query.limit as string) || DEFAULT_LIMIT,
  MAX_LIMIT
);
const offset = parseInt(req.query.offset as string) || 0;

const [data, total] = await Promise.all([
  db.query("SELECT * FROM transactions ORDER BY created_at DESC LIMIT ? OFFSET ?", [limit, offset]),
  db.query("SELECT COUNT(*) as total FROM transactions"),
]);

res.json({
  data,
  meta: { total: total[0].total, limit, offset },
});
```

## Key Takeaways

1. **Always set a default limit.** An API without a default page size is an accident waiting to happen.
2. **Always cap the maximum.** Even with a default, clients can override it. Cap at a reasonable ceiling (100–500 for most use cases).
3. **Return pagination metadata.** `{ data, meta: { total, limit, offset } }` lets clients build proper UIs without guessing.
4. **Cursor pagination > offset for huge datasets.** For tables with millions of rows, offset-based pagination gets slow (O(n) offset cost). Keyset/cursor pagination is better for truly large data. Offset is fine for moderate-sized tables.

## Resources

- [5 API Pagination Techniques You Must Know (2026)](https://www.getknit.dev/blog/api-pagination-techniques) — Knit's overview of cursor vs offset pagination
- [Pagination Best Practices in REST API Design](https://www.speakeasy.com/api-design/pagination) — Speakeasy on limit/offset patterns and response formats
- [Handling API Pagination: Fetching Large Data Sets](https://api7.ai/learning-center/api-101/handling-api-pagination) — API7.ai on defaults, total counts, and cursor security
- [API pagination best practices — Stack Overflow](https://stackoverflow.com/questions/13872273/api-pagination-best-practices) — Community discussion on keyset vs offset tradeoffs

---

*Fixed in [qepo17/koin PR #150](https://github.com/qepo17/koin/pull/150)*
