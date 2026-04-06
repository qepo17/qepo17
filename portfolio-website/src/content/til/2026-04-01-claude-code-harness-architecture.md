---
title: "6 Pillars of Claude Code's Harness Architecture (from the Source Leak)"
date: 2026-04-01
category: ai
source: neuro
---

Sebastian Raschka [analyzed the leaked Claude Code TypeScript codebase](https://sebastianraschka.com/blog/2026/claude-code-secret-sauce.html) (~512K lines, ~1,900 files) after it was [accidentally exposed via an npm source map file](https://www.theregister.com/2026/03/31/anthropic_claude_code_source_code/). The breakdown reveals 6 architectural pillars that make Claude Code effective — and the key insight is that **the harness is the secret sauce, not the model**.

## The 6 Pillars

1. **Live Repo Context** — Automatically loads git branch, recent commits, and `CLAUDE.md` at session start. The model always knows where it is.

2. **Aggressive Prompt Caching** — Boundary markers separate static from dynamic content. Static parts are cached globally across calls, cutting costs and latency.

3. **Dedicated Tools** — Custom Grep, Glob, and LSP tools (not bash wrappers). The LSP integration gives call hierarchy and cross-file references — much richer than grep alone.

4. **Context Bloat Management** — File-read deduplication, large outputs written to disk with only a preview in context, plus auto-truncation and compaction when the window fills up.

5. **Structured Session Memory** — Markdown files per conversation tracking: task spec, touched files, workflow state, errors, learnings, and a worklog.

6. **Forks & Subagents** — Parallel work via forked agents that reuse the parent's prompt cache and are aware of mutable state.

## The Core Insight

> Better harness + mediocre model > bad harness + best model.

This confirms a pattern we keep seeing: drop Claude's harness onto other models (DeepSeek, MiniMax, Kimi) with some optimization, and you get surprisingly strong coding performance. The model matters less than the scaffolding around it.

Pillar #4 (context bloat management) is particularly interesting — it's essentially what systems like OpenClaw's LCM plugin solve: keeping the context window useful without drowning it in stale content.
