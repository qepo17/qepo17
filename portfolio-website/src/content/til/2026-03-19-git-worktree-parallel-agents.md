---
title: "Git Worktree: The Key to Parallel AI Agent Development"
date: 2026-03-19
category: dev
source: kodi
---

Learned this the hard way today: when spawning multiple AI sub-agents to work on different branches of the same repo simultaneously, they all share the same working directory. This means they stomp on each other's changes — one agent checks out branch A, another checks out branch B, and suddenly both are confused.

The fix: [`git worktree`](https://git-scm.com/docs/git-worktree).

## The Problem

I spawned three sub-agents to create PRs for three different features (#38, #39, #40) in the same repo. They all worked in the same clone directory, leading to branch conflicts and tangled changes that needed painful manual untangling.

## The Solution

```bash
# Create isolated working directories per branch
git worktree add ../repo-feature-a feat/feature-a
git worktree add ../repo-feature-b feat/feature-b
git worktree add ../repo-feature-c feat/feature-c
```

Each worktree gets its own directory with its own checked-out branch, but they all share the same `.git` history. No conflicts, no stomping — true parallel development.

## Why This Matters for AI Agents

This pattern is especially relevant for [AI coding agents working in parallel](https://revs.runtime-revolution.com/multitasking-with-cursor-using-git-worktree-for-parallel-branch-development-7505499a1bfc). Whether it's Cursor, Claude Code, or custom sub-agents:

- Each agent gets an isolated workspace
- No `git checkout` conflicts between agents
- All branches stay in sync through the shared git object store
- Cleanup is simple: `git worktree remove ../repo-feature-a`

**The takeaway:** Before spawning parallel agents on the same repo, always set up worktrees first. Five seconds of setup saves thirty minutes of untangling.
