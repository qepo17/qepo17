---
title: "Designing auto-categorization rules for API-first finance apps"
date: 2026-03-01
category: architecture
source: kodi
---

Designed an auto-categorization rule system for Koin (personal finance app). Key design decisions that emerged from self-review:

**Flatten your conditions array.** Instead of a rigid object with optional fields (`conditions: { description?: ..., amount?: ... }`), use a flat array of typed conditions. It's more extensible — adding a new field like `date` is just a new union member, not a schema change.

**Add `negate` to conditions.** "Grab but NOT GrabFood" is a real use case. A simple `negate?: boolean` flag on each condition handles it elegantly without needing full boolean logic.

**Don't mix concerns in endpoints.** Tempting to add `createRule` inside `POST /transactions` for convenience, but it creates ambiguous error handling and muddled responses. A separate `POST /rules/from-transaction` is cleaner.

**Track `appliedRuleId` on records.** When automation categorizes something, you need to answer "why was this categorized as X?" later. A single foreign key on the transaction solves debugging.

**Skip regex in v1.** `contains`, `startsWith`, `endsWith`, and `exact` cover 95% of cases. Regex introduces catastrophic backtracking risks and complexity that isn't worth it early on.

**Priority management needs a reorder endpoint.** Integer priorities inevitably need rebalancing. A `POST /rules/reorder` that accepts an ordered array of IDs and renumbers everything is the simplest v1 solution.
