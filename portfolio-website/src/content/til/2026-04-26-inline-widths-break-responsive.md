---
title: "Removing Inline Widths for Responsive CSS"
date: 2026-04-26
category: "frontend"
source: "kodi"
---

A CSS redesign looked perfect on desktop but broke on mobile. The culprit wasn't missing media queries — it was `style="width: 260px"` hardcoded in HTML that overrode every responsive rule.

## The Problem

Legacy HTML had inline widths on inputs, selects, and table cells:

```html
<!-- These inline styles have higher specificity than class-based CSS -->
<input style="width: 260px" class="filter-input">
<select style="max-width: 180px" class="hero-select">
<td><input style="width: 80px" class="qty-input"></td>
```

CSS specificity hierarchy (highest to lowest):
1. Inline styles (`style="..."`) — **wins over everything**
2. IDs (`#id`)
3. Classes (`.class`), attributes (`[attr]`), pseudo-classes (`:hover`)
4. Elements (`div`, `input`)

So `.filter-input { width: 100%; }` had no effect — the inline `width: 260px` always won.

## The Fix

**Step 1: Remove all inline widths**

```html
<!-- Before -->
<input style="width: 260px" class="filter-input">

<!-- After -->
<input class="filter-input">
```

**Step 2: Add responsive CSS**

```css
/* Desktop: natural width */
.filter-input {
  width: auto;
}

/* Tablet: 2-column grid */
@media (max-width: 768px) {
  .filter-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }
}

/* Mobile: full width */
@media (max-width: 480px) {
  .filter-input,
  .hero-select,
  .qty-input {
    width: 100% !important;
  }
  
  .filter-row {
    grid-template-columns: 1fr;
  }
}
```

## The Lesson

**Inline styles are the enemy of responsive design.** They have the highest specificity and can't be overridden by normal CSS classes. Any `style="width:..."` or `style="max-width:..."` in your HTML will silently break your media queries.

**Audit your HTML for these patterns:**
- `style="width:` — any inline width
- `style="max-width:` — any inline max-width  
- `width="..."` — old HTML width attributes on tables/images

Move all sizing to CSS classes. The only inline styles that belong in HTML are dynamic values set by JavaScript.

## Source

- [MDN: CSS Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)
- [CSS-Tricks: Specifics on CSS Specificity](https://css-tricks.com/specifics-on-css-specificity/)
