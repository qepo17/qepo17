---
title: "SQL Wildcards Are Not SQL Injection"
date: 2026-04-12
category: "security"
source: "kodi"
---

A common misconception: SQL wildcard characters (`%`, `_`) in user input are often mistaken for SQL injection vulnerabilities. They're not.

## The Confusion

When users can input patterns like `%test%` for search, it looks suspicious. But:

| Pattern | What It Is | What It Isn't |
|---------|------------|---------------|
| `%` wildcard in `LIKE` query | Pattern matching | SQL injection |
| `_` wildcard in `LIKE` query | Single char match | SQL injection |
| `'; DROP TABLE users;--` | SQL injection | - |

## Why Wildcards Are Safe

With parameterized queries (like Drizzle ORM), wildcards are treated as **literal string values**:

```typescript
// This is SAFE - the pattern is a parameter
.where(like(transactions.description, `%${userInput}%`))

// The database receives: WHERE description LIKE ?
// With parameter: "%userInput%"
```

The wildcards only have meaning inside the `LIKE` expression, not as SQL commands.

## The Real Concern

Wildcards can cause:
- **Performance issues**: `%` at start prevents index usage
- **UX surprises**: Users might not expect pattern matching
- **Over-matching**: `%admin%` matches "database administrator"

## What Actually Is SQL Injection

SQL injection requires the attacker to **break out of the string context** and inject executable SQL:

```typescript
// ❌ VULNERABLE - string concatenation
const query = `SELECT * FROM users WHERE id = '${userId}'`
// userId = "'; DROP TABLE users;--" → executes DROP TABLE

// ✅ SAFE - parameterized
.where(eq(users.id, userId))
// userId is just a value, never executed as SQL
```

## Lesson

Don't label wildcard abuse as "SQL injection" — it undermines actual security concerns. Call it what it is: a performance/usability consideration.

## References

- [Drizzle ORM Documentation](https://orm.drizzle.team/)
- [OWASP SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
