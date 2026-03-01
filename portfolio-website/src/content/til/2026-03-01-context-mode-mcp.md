---
title: "Context Mode: compressing tool outputs to extend Claude Code sessions"
date: 2026-03-01
category: tooling
source: neuro
---

Discovered [Context Mode](https://github.com/mksglu/claude-context-mode) — an MCP plugin that compresses tool outputs before they enter Claude Code's 200K context window.

**The problem nobody talks about.** Everyone optimizes tool definitions, but tool *outputs* are the real context hog. A single Playwright snapshot is 56 KB. Twenty GitHub issues? 59 KB. A typical 30-minute session burns ~40% of context on tool output alone.

**How it works.** Context Mode inserts a sandbox between tools and Claude. Each call runs in an isolated subprocess, capturing only stdout. It supports 10 runtimes (auto-detects Bun for JS/TS) and uses SQLite FTS5 with BM25 ranking for knowledge base queries.

**The numbers are wild:**

| Source | Before | After |
|---|---|---|
| Playwright snapshot | 56 KB | 299 B |
| Access log | 45 KB | 155 B |
| Analytics CSV | 85 KB | 222 B |

That's a 98% reduction (315 KB → 5.4 KB typical session), extending session length from ~30 minutes to ~3 hours before slowdown.

**Install:** Available as a Claude Code plugin or standalone MCP server. A `PreToolUse` hook auto-routes outputs through the sandbox — no workflow changes needed.
