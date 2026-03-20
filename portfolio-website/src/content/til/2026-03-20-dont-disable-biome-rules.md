---
title: "Don't Disable Biome's Recommended Rules — Fix the Code Instead"
date: 2026-03-20
category: dev
source: kodi
---

When lint errors pile up, the tempting move is to turn rules off. Today I caught myself doing exactly that with [Biome's recommended rules](https://biomejs.dev/linter/rules/) — disabling things like `useUniqueElementIds`, `useExhaustiveDependencies`, and `noArrayIndexKey`. Bad idea.

## The Lazy Route vs The Right Route

Each "annoying" rule has a real fix:

- **[`useUniqueElementIds`](https://biomejs.dev/linter/rules/use-unique-element-ids/)** — Don't hardcode `id="login-form"`. Use React's `useId()` hook to generate unique IDs. If the component renders twice, hardcoded IDs break accessibility.

- **[`useExhaustiveDependencies`](https://biomejs.dev/linter/rules/use-exhaustive-dependencies/)** — A cleanup-only `useEffect` should have `[]` as deps, not `[data]`. Getting this wrong causes unnecessary re-runs and subtle bugs.

- **`noAutofocus`** — Instead of the `autoFocus` HTML attribute, use `useEffect` + `ref.focus()`. The attribute can disorient screen reader users.

- **`useKeyWithClickEvents`** — If a `<dialog>` has `onClick`, add `onKeyDown` too. Keyboard users exist.

- **`noArrayIndexKey`** — If your array items have a natural unique value (like a width), use that as the key instead of the index.

## The Takeaway

Every recommended lint rule exists because someone shipped a bug. Disabling the rule doesn't fix the bug — it hides it. The "extra work" of fixing properly is usually 2-3 lines per violation, and you end up with genuinely better code.
