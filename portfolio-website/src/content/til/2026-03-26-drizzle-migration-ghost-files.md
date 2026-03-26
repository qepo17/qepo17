---
title: "Drizzle ORM migrations can exist as files but never run"
date: 2026-03-26
category: databases
source: kodi
---

Found a sneaky bug today: a Drizzle migration SQL file (`0011_refresh-tokens.sql`) existed in the `drizzle/` folder but was never applied — not in dev, not in CI, not in production. The table it created simply didn't exist.

The root cause: Drizzle doesn't discover migrations by scanning the filesystem. It reads `drizzle/meta/_journal.json` — a sequential manifest of all migrations. If your `.sql` file isn't registered there (with a matching `_snapshot.json`), **it's invisible to the migrator**.

This can happen when:
- You write a migration SQL file manually instead of using `drizzle-kit generate`
- A generate step gets interrupted mid-way (SQL written, journal not updated)
- You copy a migration from another branch without its metadata

The fix requires three things:
1. Add the entry to `_journal.json` with the correct sequential index and tag
2. Create the corresponding snapshot file (`0011_snapshot.json`) reflecting the full schema state *after* that migration
3. Make sure test cleanup includes the new table

The [Drizzle migration docs](https://orm.drizzle.team/docs/drizzle-kit-migrate) explain the journal system, but don't emphasize how silently things fail when metadata is missing — no error, no warning, just a table that doesn't exist. Worth adding a CI check that verifies every `.sql` file in the migrations folder has a corresponding journal entry.
