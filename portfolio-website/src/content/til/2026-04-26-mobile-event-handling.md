---
title: "Why Inline onclick Fails on Mobile (and How to Fix It)"
date: 2026-04-26
category: frontend
source: kodi
---

Debugging a hamburger menu that worked on desktop but not on mobile. The button used inline `onclick="toggleMenu()"` and an icon rendered via `data-lucide="menu"` (requires JS to render).

**What went wrong:**

1. **Lucide icons render via JS** — `data-lucide="menu"` needs the Lucide library to swap the attribute for an SVG. On slow networks, the button appears empty and unclickable until the JS loads.
2. **Inline `onclick` is unreliable on mobile** — Some mobile browsers block inline event handlers, or touch events don't map cleanly to clicks. The `onclick` attribute that works on desktop silently fails on touch devices.

**The fixes:**

```javascript
// Instead of inline onclick, use addEventListener with both click + touchend
document.querySelectorAll('.menu-toggle').forEach(btn => {
  btn.addEventListener('click', toggleMenu);
  btn.addEventListener('touchend', (e) => {
    e.preventDefault();
    e.stopPropagation();
    toggleMenu();
  });
});
```

```html
<!-- Instead of data-lucide (JS-dependent), use inline SVG -->
<button class="menu-toggle">
  <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2">
    <line x1="3" y1="6" x2="21" y2="6"></line>
    <line x1="3" y1="12" x2="21" y2="12"></line>
    <line x1="3" y1="18" x2="21" y2="18"></line>
  </svg>
</button>
```

**Additional mobile-specific fixes:**
- `min-width: 40px; min-height: 40px` on the button for proper tap target size
- Auto-close when tapping outside the menu (detect clicks on the document)
- `display: block` on the SVG to prevent layout collapse

**Key insight:** Mobile debugging isn't just "make it smaller." Touch events and click events are different APIs with different reliability guarantees. If something works on desktop but not mobile, the first thing to check isn't the CSS — it's whether the event is firing at all.

Source: [MDN - Touch Events](https://developer.mozilla.org/en-US/docs/Web/API/Touch_events)
