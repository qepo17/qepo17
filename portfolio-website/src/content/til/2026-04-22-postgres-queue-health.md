---
title: "Keeping a Postgres Queue Healthy"
date: 2026-04-22
category: "databases"
source: "neuro"
---

# Keeping a Postgres Queue Healthy

PlanetScale published a practical guide on [keeping Postgres job queues healthy](https://planetscale.com/blog/keeping-a-postgres-queue-healthy) — specifically dealing with the silent degradation that happens when `VACUUM` falls behind on high-churn tables.

## The Problem

Job queues are high-churn tables: rows are inserted, updated, and deleted rapidly. This creates **dead tuples** — old row versions that Postgres keeps for MVCC (Multi-Version Concurrency Control). Dead tuples need to be cleaned up by `VACUUM`, but:

- High write volume generates dead tuples faster than VACUUM can clean them
- Long-running transactions block VACUUM from removing dead tuples
- Autovacuum may not keep up with aggressive queue workloads
- Dead tuple bloat degrades query performance and wastes disk space

## The Solutions

**1. Monitor dead tuple ratio**
```sql
SELECT schemaname, relname, n_dead_tup, n_live_tup, 
       ROUND(n_dead_tup * 100.0 / NULLIF(n_live_tup + n_dead_tup, 0), 2) AS dead_pct
FROM pg_stat_user_tables 
WHERE relname = 'your_queue_table';
```

**2. Tune autovacuum for the queue table**
```sql
ALTER TABLE job_queue SET (
  autovacuum_vacuum_scale_factor = 0.05,
  autovacuum_vacuum_threshold = 50,
  autovacuum_analyze_scale_factor = 0.02
);
```

**3. Use `SKIP LOCKED` for consumers**
```sql
SELECT * FROM job_queue 
WHERE status = 'pending' 
ORDER BY created_at 
FOR UPDATE SKIP LOCKED 
LIMIT 1;
```
This prevents consumers from blocking each other while polling.

**4. Consider `pg_boss` or `graphile-worker`** instead of rolling your own queue — they're designed to handle these Postgres-specific issues.

---

*Lesson learned: Postgres can be a great job queue, but only if you monitor and tune for the specific pathology of high-churn tables. Dead tuple bloat is silent until it isn't — by then your queue latency has already spiked.*
