---
title: "Git PR Anti-Pattern: Accidentally Deleting Files From Main"
date: 2026-04-12
category: "git"
source: "kodi"
---

A subtle git workflow mistake: creating files on a branch that already exist in main, then deleting them, makes Git think you want to delete them from the repository.

## The Scenario

1. Branch A creates `file.txt` → merged to main
2. You create Branch B, also creating `file.txt` (not knowing it exists)
3. Git shows modifications, not new file
4. You `git rm file.txt` to "clean up"
5. PR shows: **-1 file** (deletion!)

## Why This Happens

Git tracks content, not just filenames. When two branches create identical/similar files, Git may treat them as the same file with modifications.

```bash
# Your branch shows modifications
git status
# modified:   file.txt   ← not "new file"

# You think: "I don't need this"
git rm file.txt

# Git thinks: "They want to delete file.txt from main"
```

## The Fix

**Don't delete** — restore from main:

```bash
# Checkout the file from main branch
git checkout main -- file.txt

# Or restore all files that exist in main
git checkout main -- .

# Commit the restoration
git commit -m "Restore files from main"
```

## Prevention

1. **Always pull main before starting new work:**
   ```bash
   git checkout main
   git pull
   git checkout -b new-branch
   ```

2. **Check if files exist before creating:**
   ```bash
   git ls-tree main -- path/to/file
   ```

3. **Review your PR diff carefully** — watch for unexpected deletions

## What "Rebase From Main" Means

When someone says "rebase from main":

```bash
# Update your local main
git checkout main
git pull origin main

# Rebase your branch
git checkout your-branch
git rebase main

# Force push to update PR
git push --force-with-lease
```

This replays your commits on top of the latest main, avoiding conflicts and incorporating recent changes.

## Lesson

In PR diffs, **deletions are dangerous**. Always verify that deleted files are intentional — especially if you never meant to touch them in the first place.
