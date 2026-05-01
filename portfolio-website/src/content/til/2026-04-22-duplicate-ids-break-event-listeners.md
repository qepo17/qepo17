---
title: "Theme Toggle Bug: When Duplicate IDs Break Event Listeners"
date: 2026-04-22
category: "frontend"
source: "kodi"
---

A dark mode toggle that worked on desktop but failed on mobile — the classic "works on my machine" bug. The root cause was a fundamental DOM API misunderstanding.

## The Problem

`ThemeToggle` was rendered twice: once in the desktop navigation and once in the mobile header. The original code used `getElementById` to attach the click listener:

```javascript
const btn = document.getElementById('theme-toggle');
btn.addEventListener('click', toggleTheme);
```

`getElementById` returns **only the first match** in the DOM. So the desktop button got the listener, but the mobile button didn't. Clicking the mobile hamburger menu's theme button did nothing.

## The Fix

Switch from `id` to `class` and use `querySelectorAll` to attach listeners to **all** instances:

```astro
<!-- Before: unique id (only first instance works) -->
<button id="theme-toggle">
  <span id="theme-toggle-sun">☀️</span>
  <span id="theme-toggle-moon">🌙</span>
</button>

<!-- After: class (all instances work) -->
<button class="theme-toggle">
  <span class="theme-toggle-sun">☀️</span>
  <span class="theme-toggle-moon">🌙</span>
</button>
```

```javascript
// Before: only catches the first button
const btn = document.getElementById('theme-toggle');

// After: catches both desktop and mobile buttons
document.querySelectorAll('.theme-toggle').forEach(btn => {
  btn.addEventListener('click', toggleTheme);
});

// Update all icon sets in sync
document.querySelectorAll('.theme-toggle-sun').forEach(el => el.classList.toggle('hidden'));
document.querySelectorAll('.theme-toggle-moon').forEach(el => el.classList.toggle('hidden'));
```

## Key Takeaways

- **`id` must be unique per page** — browsers enforce this by only returning the first match
- **Components rendered multiple times need classes** — navbars, footers, sidebars often duplicate UI elements
- **`querySelectorAll` + `forEach`** is the pattern for multi-instance components
- **Both icon sets must update** — otherwise desktop shows sun while mobile shows moon after a toggle

## Source

- [MDN: getElementById](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById) — "If there are multiple elements with the same ID, only the first is returned"
- [MDN: HTML id attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id) — "must be unique in the whole document"
