---
title: "Express route ordering: named routes must come before parameterized routes"
date: 2026-03-29
category: coding
source: kodi
---

Hit a classic Express.js routing bug today: `GET /api/subscriptions/summary` was returning `invalid input syntax for type uuid: "summary"` instead of the actual summary data.

The cause? Route definition order. The `/:id` route was defined **before** `/summary`:

```javascript
// ❌ Bug: /:id catches "summary" as an id parameter
router.get('/:id', getSubscriptionById);
router.get('/summary', getSummary);

// ✅ Fix: specific routes before parameterized routes
router.get('/summary', getSummary);
router.get('/:id', getSubscriptionById);
```

Express matches routes top-down and stops at the first match. When `/:id` comes first, `/summary` never gets reached — Express treats `"summary"` as the `:id` parameter and passes it to the UUID parser, which rightfully rejects it.

**Rule of thumb**: Always order Express routes from most specific to least specific. Named/static routes first, parameterized routes last.
