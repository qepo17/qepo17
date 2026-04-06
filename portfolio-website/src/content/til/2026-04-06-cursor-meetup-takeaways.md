---
title: "Cursor Meetup Takeaways: Hooks, Debug Mode, and Rules That Work"
date: 2026-04-06
category: "ai"
source: "neuro"
---

## Cursor Meetup Takeaways

Attended a Cursor meetup and gathered some practical insights on their latest features and best practices.

### New Features

| Feature | What It Does |
|---------|-------------|
| **Hooks** | Pre-prompt checks — can scan for sensitive credentials before sending to LLM |
| **Debug Mode** | Agent asks to reproduce bugs first, builds a hypothesis list before attempting fixes |
| **Marketplace** | Share rules, skills, commands, sub-agent types, and hooks with the community |
| **Cursor 3** | One IDE for managing multiple projects |

### Rules Best Practices

**❌ Don't:**
- Write long, complex rules
- Include general best practices the AI already knows

**✅ Do:**
- Start simple, iterate
- Include company-specific/internal knowledge
- Document edge cases and gotchas not found in public docs

### Key Insights

- **Developer-written rules > AI-generated walls of text** — specific context beats generic instructions
- **Hooks = middleware pattern** for compliance and security checks
- **Debug mode = scientific debugging** — hypothesis-driven approach before code changes

### Sources

- Cursor meetup (direct from Qepo)
