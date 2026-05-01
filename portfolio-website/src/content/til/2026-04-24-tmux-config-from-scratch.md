---
title: "Tmux Config from Scratch: Mouse, Vim Keys, and Sensible Defaults"
date: 2026-04-24
category: "devops"
source: "kodi"
---

A complete, minimal tmux configuration that covers the essentials: mouse support, vim-style navigation, intuitive splits, and a clean status bar. No plugins needed.

## The Full Config

```tmux
# ── Prefix ──────────────────────────────────────────────
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# ── Mouse Support ─────────────────────────────────────
set -g mouse on

# ── Terminal & Colors ─────────────────────────────────
set -g default-terminal "tmux-256color"
set -ag terminal-overrides ",xterm-256color:RGB"

# ── Windows & Panes ───────────────────────────────────
set -g base-index 1
setw -g pane-base-index 1
set -g renumber-windows on

# Intuitive splits (| and - instead of " and %)
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"
bind c new-window -c "#{pane_current_path}"

# Reload config with Prefix + r
bind r source-file ~/.tmux.conf \; display "Config reloaded!"

# ── Navigation ────────────────────────────────────────
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

bind -r H resize-pane -L 5
bind -r J resize-pane -D 5
bind -r K resize-pane -U 5
bind -r L resize-pane -R 5

# ── Copy Mode ─────────────────────────────────────────
setw -g mode-keys vi
bind-key -T copy-mode-vi v send-keys -X begin-selection
bind-key -T copy-mode-vi y send-keys -X copy-selection-and-cancel

# ── Status Bar ────────────────────────────────────────
set -g status-interval 5
set -g status-style bg=black,fg=white
set -g status-left "#[fg=green]#{session_name} #[fg=white]| "
set -g status-right "#[fg=cyan]%H:%M #[fg=white]%d-%b-%y"

# ── Misc ──────────────────────────────────────────────
set -g history-limit 50000
set -g escape-time 0
set -g focus-events on
setw -g aggressive-resize on
```

## Key Design Decisions

**C-a over C-b** — `Ctrl+a` is easier to reach than `Ctrl+b` (especially on keyboards where `b` requires a stretch). If you use `screen`, you're already used to it.

**Mouse on** — Click to select panes, drag to resize, scroll with the wheel. Purists disable it; pragmatists enable it. Modern tmux mouse integration doesn't interfere with terminal selection when you hold Shift.

**`-c "#{pane_current_path}"`** — New splits/windows open in the same directory as the current pane. Saves constant `cd`ing.

**`renumber-windows on`** — Close window 3 in a 5-window session, and windows 4-5 become 3-4. Keeps window numbers contiguous and predictable.

**`escape-time 0`** — Removes the delay after pressing Escape (crucial for vim users). Without this, exiting insert mode feels sluggish.

## Source

- [tmux GitHub Wiki: Configuration Examples](https://github.com/tmux/tmux/wiki/Configuration-Examples)
- [Ham Vocke: A Gentle Guide to tmux](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)
