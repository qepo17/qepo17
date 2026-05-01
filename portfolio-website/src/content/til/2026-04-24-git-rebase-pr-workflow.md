---
title: "Git Rebase for PRs: Keeping Feature Branches Clean"
date: 2026-04-24
category: "git"
source: "kodi"
---

Rebasing a feature branch onto the latest main before merge keeps history linear and avoids merge commit noise. Here's the workflow I use for PRs.

## The Scenario

You created a PR branch a few days ago. Meanwhile, `main` has moved forward with new commits. GitHub shows "This branch is out-of-date with the base branch."

## The Commands

```bash
# 1. Fetch latest main
 git fetch origin main

# 2. Checkout your feature branch
git checkout til-2026-04-22

# 3. Rebase onto latest main
git rebase origin/main

# 4. Force-push to update the PR
git push --force-with-lease origin til-2026-04-22
```

## Why Rebase Instead of Merge?

| Approach | Result |
|----------|--------|
| **Merge main into branch** | Creates a merge commit; history becomes a tangle of criss-crossing lines |
| **Rebase branch onto main** | Replays your commits on top of latest main; history stays linear |

Linear history makes `git bisect` work better, `git log` readable, and rollback predictable.

## What `rebase` Actually Does

```
Before rebase:
main:    A---B---C---D
branch:       \   E---F

After rebase:
main:    A---B---C---D
branch:               E'---F'
```

Commits E and F are **replayed** on top of D. They get new hashes (E', F') because their parent changed.

## Force-Push Safety

Always use `--force-with-lease` instead of `--force`:

- `--force` overwrites remote branch unconditionally (dangerous if someone else pushed)
- `--force-with-lease` only pushes if remote matches what you last fetched — fails if there are new commits you haven't seen

## When NOT to Rebase

- **Shared branches** — rebasing rewrites history; anyone else working on the branch will have conflicts
- **Already-merged PRs** — the commits are public; rewriting them breaks references
- **Long-lived branches with many commits** — resolving conflicts for each replayed commit becomes tedious

## Source

- [Git Documentation: git-rebase](https://git-scm.com/docs/git-rebase)
- [GitHub Docs: About Git rebase](https://docs.github.com/en/get-started/using-git/about-git-rebase)
