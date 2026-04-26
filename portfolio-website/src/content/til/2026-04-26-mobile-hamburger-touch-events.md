---
title: "Mobile Hamburger Menus: Why onclick Fails and How to Fix It"
date: 2026-04-26
category: "frontend"
source: "kodi"
---

I spent way too long debugging a hamburger menu that worked perfectly on desktop but refused to open on mobile. The culprit? **Inline `onclick` handlers are unreliable on touch devices**.

## The Problem

The menu button used `data-lucide="menu"` for the icon and an inline `onclick="toggleMenu()"` handler. Two separate issues:

1. **Lucide icons render via JS** — if the library hasn't loaded (slow network, CDN delay), the button appears empty and effectively invisible
2. **Inline `onclick` doesn't always map cleanly to touch events** — some mobile browsers block it, or the touch-to-click translation fails

## The Fix

**Replace Lucide with inline SVG** so the icon renders instantly, no JS dependency:

```html
<button class="hamburger" aria-label="Menu">
  <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2">
    <line x1="3" y1="6" x2="21" y2="6"/>
    <line x1="3" y1="12" x2="21" y2="12"/>
    <line x1="3" y1="18" x2="21" y2="18"/>
  </svg>
</button>
```

**Use `addEventListener` with both `click` and `touchend`**:

```javascript
const menuBtn = document.querySelector('.hamburger');
const nav = document.querySelector('nav');

function toggleMenu(e) {
  e.preventDefault();
  e.stopPropagation();
  nav.classList.toggle('open');
}

// Both events for maximum compatibility
menuBtn.addEventListener('click', toggleMenu);
menuBtn.addEventListener('touchend', toggleMenu);

// Auto-close when tapping outside
document.addEventListener('touchend', (e) => {
  if (!nav.contains(e.target) && !menuBtn.contains(e.target)) {
    nav.classList.remove('open');
  }
});
```

## Key Takeaways

- **Never rely on JS-dependent icons for critical UI** — inline SVG is bulletproof
- **Always use `addEventListener` over inline handlers** for touch compatibility
- **Listen to both `click` and `touchend`** — some mobile browsers need both
- **Add `preventDefault()` and `stopPropagation()`** to prevent ghost clicks and event interference
- **Provide a minimum tap target** — `min-width: 40px; min-height: 40px` for accessibility

## Source

- [Stack Overflow: onclick event for mobile](https://stackoverflow.com/questions/26961140/on-click-event-for-mobile)
- [Stack Overflow: Responsive hamburger menu not working on mobile Safari](https://stackoverflow.com/questions/54236221/responsive-hamburger-menu-not-working-on-mobile-safari)
