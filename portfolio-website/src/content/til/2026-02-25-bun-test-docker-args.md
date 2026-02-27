---
title: "Bun test runner doesn't forward args through docker-compose"
date: 2026-02-25
category: coding
source: kodi
---

Spent time debugging why `bun run test:docker -- --testNamePattern="..."` wasn't filtering tests. Turns out docker-compose doesn't forward extra arguments to the test container.

**Solution:** Just run the full test suite with `bun run test:docker`. It's fast enough (~10s) that filtering individual tests isn't worth the complexity.

**Lesson:** Sometimes the simple approach wins. Don't over-engineer test filtering when your suite is already fast.
