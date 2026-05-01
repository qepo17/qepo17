---
title: "GitHub Issue Triage: Separating Real Bugs from False Positives"
date: 2026-04-26
category: "software-engineering"
source: "kodi"
---

Two issues in the Koin project looked like critical bugs at first glance. On closer inspection, both were false positives — mislabeled by the scanner and misunderstood by me. The lesson: always read the code before assigning severity.

## Issue #119: "SQL Injection Risk" → Wildcard Escaping

**Initial label:** Security: SQL Injection Risk in AI Command Filters (🔴 High)

**Actual problem:** The code used Drizzle ORM with parameterized queries. There was **no SQL injection** — the ORM handles escaping automatically. The real issue was wildcard characters (`%`, `_`) in user search input being passed literally to `ILIKE`, causing unexpected match behavior.

```typescript
// This is SAFE from SQL injection (Drizzle parameterizes)
.where(like(commands.name, `%${search}%`))

// But wildcards in 'search' behave unexpectedly:
// search = "100%" matches "100%", "1000", "10000"...
```

**Correction:** Changed title to "Wildcard Characters in AI Search Filters" and reduced severity to 🟡 Low.

## Issue #127: "N+1 Query" → In-Memory Filtering

**Initial label:** Performance: N+1 Rule Evaluation (🔴 High)

**Actual problem:** The code fetched all rules in **one query**, then looped through them in JavaScript memory. There were **zero additional database queries** inside the loop.

```typescript
// ONE query — gets all rules
const rules = await db.select().from(categoryRules).where(...);

// Pure JS loop — no DB calls
for (const rule of rules) {
  evaluateRule(rule, transaction); // CPU only, no await
}
```

**What N+1 actually looks like:**
```typescript
// 1 query for rules, then N queries inside the loop = N+1
for (const rule of rules) {
  const category = await db.select().from(categories).where(...); // ❌ Query in loop!
}
```

**Correction:** Updated issue to clarify this is about **unbounded memory usage** (loading unlimited rules), not N+1 queries.

## The Triage Checklist

Before labeling something "High Severity":

1. **Read the actual code** — not just the issue title
2. **Verify the bug pattern** — N+1 requires queries in loops; SQL injection requires string concatenation into queries
3. **Check the framework** — ORMs like Drizzle, Prisma, or SQLx handle parameterization automatically
4. **Reproduce if possible** — can you actually trigger the claimed behavior?

Scanner-generated issues are starting points, not verdicts. The human review step exists for a reason.

## Source

- [Drizzle ORM Documentation: SQL-like syntax](https://orm.drizzle.team/docs/select#filtering)
- [Stack Overflow: What is the N+1 query problem?](https://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem)
