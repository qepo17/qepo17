---
title: "In-Memory Filtering Is Not N+1"
date: 2026-04-12
category: "performance"
source: "kodi"
---

Loading data into memory and filtering with JavaScript is **not** the N+1 query problem. The confusion is common but important to clarify.

## What N+1 Actually Is

The N+1 query problem requires **database queries inside a loop**:

```typescript
// ❌ N+1: Query inside loop
const users = await db.select().from(users); // 1 query

for (const user of users) {
  const orders = await db.select().from(orders)
    .where(eq(orders.userId, user.id)); // N queries!
}
// Total: 1 + N queries (hence "N+1")
```

## What Is NOT N+1

Loading data once and filtering in memory:

```typescript
// ✅ NOT N+1: One query, in-memory filtering
const rules = await db
  .select()
  .from(categoryRules)
  .where(eq(categoryRules.userId, userId));

// JavaScript loop - zero DB queries
for (const rule of rules) {
  if (evaluateRule(rule, transaction)) {
    return rule;
  }
}
// Total: 1 query total
```

## The Comparison

| Pattern | Query Count | Issue |
|---------|-------------|-------|
| N+1 | 1 + N | Database overload |
| In-memory filtering | 1 | Memory usage (if data is huge) |

## When In-Memory Filtering Makes Sense

- Rules/categories fit in memory comfortably
- Complex evaluation logic that's hard to express in SQL
- Need to avoid repeated round-trips to database

## When to Avoid

- Dataset is unbounded (could load thousands of rows)
- Simple filtering that SQL handles better
- Memory-constrained environments

## The Real Issue With Unbounded Loading

If a user has 10,000 rules, loading them all into memory is a **memory usage problem**, not an N+1 problem. Consider:

- Pagination
- Database-level filtering for simple conditions
- Caching strategies

## References

- [The N+1 Database Query Problem - Medium](https://medium.com/databases-in-simple-words/the-n-1-database-query-problem-a-simple-explanation-and-solutions-ef11751aef8a)
- [What is N+1 Query Problem - PlanetScale](https://planetscale.com/blog/what-is-n-1-query-problem-and-how-to-solve-it)
- [Stack Overflow: N+1 Selects Problem](https://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem-in-orm-object-relational-mapping)
