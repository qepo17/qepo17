---
title: "CSS Cache Busting with Query Strings"
date: 2026-04-26
category: frontend
source: kodi
---

Deployed a CSS redesign (purple → blue/orange) but users still saw the old styles. The file `styles.css` was updated on the server, but browsers served the cached version.

**Why this happens:**
Browsers and CDNs cache CSS files aggressively by URL. When the URL stays the same (`styles.css`), the browser says "I already have this" and serves the old version — even after you push new changes.

**The fix:**
Add a query parameter to the URL:

```html
<!-- Before -->
<link rel="stylesheet" href="styles.css">

<!-- After -->
<link rel="stylesheet" href="styles.css?v=4">
```

The browser sees `styles.css?v=4` as a **different file** and fetches the fresh version.

**Versioning strategy:**
- `?v=1` → initial release
- `?v=2` → first CSS update
- `?v=3`, `?v=4` → subsequent updates

**Important:** Bump the number on **all pages** that reference the stylesheet, not just one. If `index.html` gets `?v=4` but `about.html` stays at `?v=3`, users see inconsistent styles across pages.

**Better alternatives for production:**
- Build tools (Vite, webpack) can hash the filename automatically: `styles.a3f7c2.css`
- CI/CD pipelines can inject the git commit hash as the version: `?v=abc123`

**Key insight:** Cache invalidation is one of the hard problems in computer science. Query-string versioning is the simplest manual approach, but filename hashing is more robust — it works with aggressive CDN caching and doesn't require remembering to bump numbers.

Source: [MDN - HTTP Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)
