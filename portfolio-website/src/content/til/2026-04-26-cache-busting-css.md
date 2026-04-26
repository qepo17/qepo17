---
title: "Cache Busting CSS: Why Query Strings Save Your Sanity"
date: 2026-04-26
category: "frontend"
source: "kodi"
---

After deploying a complete CSS redesign (purple L-Freight → tiket.com blue/orange), users were still seeing the old styles. The browser cache was the culprit.

## The Problem

Browsers and CDNs cache CSS files aggressively. When the URL stays the same (`styles.css`), the browser says "I already have this" and serves the old cached version — even after you pushed new changes to the server.

## The Solution: Query String Versioning

By adding `?v=4`, the URL becomes `styles.css?v=4`. The browser sees this as a **different file** and fetches the fresh version from the server.

```html
<!-- Before -->
<link rel="stylesheet" href="styles.css">

<!-- After -->
<link rel="stylesheet" href="styles.css?v=4">
```

**Standard practice:**
- `?v=1` → initial release
- `?v=2` → first CSS update
- `?v=3`, `?v=4` → subsequent updates

Every time you change `styles.css`, bump the number. This applies to all static assets — JS files, images, fonts.

## Automated Alternatives

For production apps, build tools handle this automatically:

- **Vite**: hashes the filename (`styles.a3f2b1c.css`)
- **Webpack**: content-hash in filename
- **Vercel/Netlify**: immutable cache headers with hashed assets

## The Lesson

**Never deploy CSS/JS changes without cache busting.** Users will silently get stale assets, and you'll waste hours debugging "why isn't my fix working?" when it is working — just not for anyone with the old file cached.

## Source

- [MDN: HTTP Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)
