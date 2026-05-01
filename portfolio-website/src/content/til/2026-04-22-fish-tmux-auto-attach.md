---
title: "Fish Shell + Tmux: Auto-Attach on Every Terminal Open"
date: 2026-04-22
category: "devops"
source: "kodi"
---

A simple fish config snippet that automatically attaches to a tmux session (or creates one) every time you open a terminal — except when you're already in tmux or inside VS Code's integrated terminal.

## The Config

```fish
if status is-interactive
    if not set -q TMUX
        and test "$TERM_PROGRAM" != "vscode"
        tmux new-session -A -s main
    end
end
```

## How It Works

| Condition | Purpose |
|-----------|---------|
| `status is-interactive` | Only runs for interactive shells (not scripts) |
| `not set -q TMUX` | Skip if already inside a tmux session (prevents nested tmux) |
| `test "$TERM_PROGRAM" != "vscode"` | Skip in VS Code terminal — VS Code has its own session management and tmux can interfere with integrated features |
| `tmux new-session -A -s main` | Create session "main" if it doesn't exist, or attach to it if it does (`-A` = attach if exists) |

## Why This Pattern Matters

Without the guards:
- **Nested tmux** — opening a terminal inside tmux creates a session inside a session, breaking key bindings and confusing the prefix key
- **VS Code conflicts** — the integrated terminal expects direct shell access; tmux can swallow scrollback, copy mode, and某些 extensions that depend on shell integration

## Variations

**Named sessions per project:**
```fish
set session_name (basename (pwd))
tmux new-session -A -s $session_name
```

**Fallback to plain shell if tmux isn't installed:**
```fish
if command -v tmux >/dev/null 2>&1
    tmux new-session -A -s main
end
```

## Source

- [fish shell documentation: Conditional execution](https://fishshell.com/docs/current/language.html#conditional-execution)
- [tmux man page: new-session](https://man7.org/linux/man-pages/man1/tmux.1.html) — `-A` attaches to existing session if it exists
