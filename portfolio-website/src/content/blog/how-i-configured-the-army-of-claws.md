---
title: 'How I Configured the Army of Claws'
description: A deep dive into building a multi-agent AI system with OpenClaw — where every agent has a name, a soul, and a job to do.
pubDate: 2026-03-14T00:00:00Z
heroImage: '/assets/images/posts/openclaw.png'
tags: ['ai', 'openclaw', 'agents', 'workflow', 'automation']
---

We've all been there. You ask your AI to check stock prices, then switch to parsing a bank statement, then pivot to debugging some code. Context gets muddled. The assistant tries to be everything at once and ends up being mediocre at all of it.

I wanted something different: **specialists.** An army of agents, each with a clear domain, isolated workspaces, and purpose-built skills. No jack-of-all-trades generalist soup — just clean separation of concerns, applied to AI.

That's what I built with [OpenClaw](https://openclaw.ai).

## The Architecture: One Gateway, Five Agents

OpenClaw runs as a single gateway daemon on my server, but behind that gateway sit **five distinct agents**, each with:

- Their own **identity** (name, personality, emoji)
- Their own **workspace** (isolated file system)
- Their own **skills** (installed tools tailored to their role)
- Their own **Telegram bot** (separate bot token, separate conversations)

All five agents share the same base model — **Claude Opus 4** — but what makes them different is everything *around* the model: the context they carry, the tools they can use, and the personality they embody.

Here's the lineup:

| Agent | Name | Role | Emoji |
|-------|------|------|-------|
| `main` | **Kai** | Generalist / Coordinator | 🙇 |
| `investment` | **Cuan** | Stock Market & Investment | 📈 |
| `finance` | **Claudia** | Financial Documents & Banking | 💰 |
| `research` | **Neuro** | Learning & Research | 🧠 |
| `coding` | **Kodi** | Software Development & GitHub | 💻 |

## Meet the Agents

### 🙇 Kai — The Generalist

Kai is the **default agent** and the one I talk to most. Think of Kai as the team lead — a generalist who can handle anything that doesn't need a specialist.

**Skills installed:**
- `bird` — Twitter/X integration
- `deep-research-pro` — Multi-step web research with synthesis
- `gog` — Google search integration
- `summarize` — Content summarization
- `clawhub` — Browse and install community skills
- `healthcheck` — System health monitoring
- `skill-creator` — Create new custom skills
- `tmux` — Remote terminal control
- `weather` — Weather lookups

Kai carries the **most skills** because as a generalist, Kai needs the broadest toolkit. But there's a twist: Kai also acts as a **peer reviewer** for the specialist agents. When Cuan provides stock analysis or Neuro delivers research, Kai is configured to critically review their work — spotting gaps, questioning assumptions, and challenging conclusions when warranted. It's like having a senior engineer review PRs from domain experts.

Kai's personality is defined as *"genuinely helpful, not performatively helpful"* — direct, casual, but accountable. No corporate drone energy.

### 📈 Cuan — The Investment Analyst

Cuan is my **stock market specialist**. The name "Cuan" is Indonesian slang for profit/gains — fitting for an agent that lives and breathes market data.

**Skills installed:**
- `stock-price-checker` — Real-time stock price lookups
- `blogwatcher` — Monitor investment blogs and news sources
- `deep-research-pro` — Deep research for investment due diligence
- `financial-statement-analyzer` — Analyze company financials (10-K, 10-Q reports)

**Why these skills?** Investment analysis requires a specific pipeline: check current prices, read the latest news, deep-dive into company fundamentals, and analyze financial statements. Each skill covers one step of that workflow.

Cuan is configured to be **precise and data-driven** — no speculation, no buy/sell recommendations. Just data, analysis, and timestamps so I know how fresh the information is. It's an information tool, not a financial advisor.

### 💰 Claudia — The Finance Manager

Claudia handles **personal finance** — specifically, processing bank statements and financial documents.

**Skills installed:**
- `bank-statement-parser` — Parse and extract transactions from bank statement PDFs

**Why only one skill?** Because Claudia has a *very* focused job: take a bank statement PDF, extract the transactions, and present them in a structured format. That's it. No scope creep. No feature bloat.

Claudia is **thorough and methodical** — financial data demands precision. She's also configured with a critical security practice: **delete financial PDFs after processing.** Sensitive documents don't linger on disk.

This is the purest example of the single-responsibility principle in my agent army. One agent, one job, done well.

### 🧠 Neuro — The Research Engine

Neuro is my **learning and research specialist**. When I want to understand a new technology, explore an AI paper, or stay current with tech news, Neuro is who I talk to.

**Skills installed:**
- `academic-deep-research` — Academic paper research and analysis
- `summarize` — Distill long content into key takeaways
- `blogwatcher` — Monitor tech blogs and publications
- `hackernews` — Track trending discussions on Hacker News
- `arxiv-watcher` — Monitor new papers on arXiv

**Why these skills?** Neuro's skill set covers the full research lifecycle: discover new content (arxiv-watcher, hackernews, blogwatcher), go deep on specific topics (academic-deep-research), and distill findings into digestible summaries (summarize).

Neuro is **curious and educational** — designed to break down complex concepts and support continuous learning. When I ask about transformer architectures or the latest reinforcement learning paper, Neuro doesn't just give me a surface-level answer — it digs in.

### 💻 Kodi — The Code Machine

Kodi is the **coding specialist** — software development, GitHub operations, debugging, and DevOps.

**Skills installed:**
- `openclaw-github-assistant` — Full GitHub integration (repos, PRs, issues, actions)
- `tmux` — Remote terminal session control

**Why these skills?** Code needs two things: a way to interact with repositories (GitHub) and a way to interact with running processes (tmux for terminal sessions). Everything else — the actual coding, debugging, architecture decisions — comes from the base model's capabilities, not from skills.

Kodi is configured to be **direct and practical** — code speaks louder than words. He creates feature branches, writes tests, opens PRs, and never pushes directly to main. Discipline baked into personality.

Fun fact: Kodi wrote this blog post. And the PR for it.

## The Telegram Setup: One Group, Five Topics

Here's where it gets elegant. All five agents live in a **single Telegram forum group**, but each agent is bound to its own **topic thread**:

| Agent | Topic | Purpose |
|-------|-------|---------|
| Kai | General | General conversation, coordination |
| Claudia | Finance | Finance requests |
| Cuan | Investment | Investment queries |
| Neuro | Research | Research & learning |
| Kodi | Coding | Coding tasks |

Each agent has its own **Telegram bot token**, which means they appear as separate bots in the chat. When I post in the investment topic, only Cuan responds. When I post in the coding topic, only Kodi picks it up. No cross-talk, no confusion.

I don't have to @-mention the bot — just posting in the right topic is enough. And each topic is configured as "open" within the group, while the overall group policy remains an allowlist (only I can trigger them).

**Why a forum group instead of separate DMs?** Organization. I can see all my agent interactions in one place, organized by domain. It's like Slack channels but for AI agents.

## The Infrastructure Decisions

### Isolated Workspaces

Each agent gets its own workspace directory. Every workspace contains the agent's `SOUL.md` (personality), `MEMORY.md` (long-term memory), `USER.md` (user preferences), and any project files. Isolation means:

1. **No memory bleed** — Kodi's coding memories don't pollute Cuan's investment context
2. **Focused context** — Each agent only loads files relevant to its domain
3. **Security** — Claudia's financial documents don't leak to other agents
4. **Clean SOUL files** — Each agent's personality can be tuned independently

### Model Fallbacks

All agents share the same fallback chain:

```
Primary:    Claude Opus 4
Fallback 1: Claude Sonnet 4
Fallback 2: Claude Haiku 3.5
```

If Opus is unavailable, agents gracefully degrade to Sonnet, then Haiku. This keeps the system running even during API outages.

### Context Management

Two key settings keep conversations efficient:

- **Context Pruning** (cache-TTL mode, 1h TTL) — Old context gets pruned after an hour of inactivity, preventing token waste on stale conversations
- **Compaction** (safeguard mode) — When context gets too large, it's intelligently compacted rather than brutally truncated

I also run the **Lossless Claw** plugin which provides lossless context management — compacted context can be expanded back on demand, so nothing is truly lost.

### Concurrency

- **Max concurrent sessions per agent:** 4
- **Max concurrent sub-agents:** 8

This means agents can spawn sub-tasks without blocking their main conversation, and the system won't spiral into unbounded resource usage.

## Why Single-Responsibility Agents?

The philosophy is borrowed directly from software engineering: **the Single Responsibility Principle.**

A class should have only one reason to change. An agent should have only one domain to master.

When Cuan only thinks about stocks, its workspace memory is 100% investment context. Its skills are 100% market-relevant. There's no "oh wait, let me switch from analyzing BBCA.JK to debugging a TypeScript error" whiplash.

The benefits:

1. **Better context utilization** — Every token of memory is domain-relevant
2. **Cleaner personalities** — Each agent's SOUL.md is tuned for its role
3. **Easier debugging** — If stock analysis is wrong, I check Cuan. If code is broken, I check Kodi.
4. **Security boundaries** — Financial data stays in the finance agent
5. **Parallel work** — I can ask Neuro to research something while Kodi writes code, simultaneously

The only agent that breaks this rule is Kai — and that's by design. Every team needs a generalist who can handle the stuff that doesn't fit neatly into a box.

## The Result

What I ended up with is essentially a **personal AI department**:

- **Kai** is the team lead and jack-of-all-trades
- **Cuan** is the market analyst
- **Claudia** is the accountant
- **Neuro** is the researcher
- **Kodi** is the developer

All running on one server, one OpenClaw gateway, one Telegram group. Each agent knows who it is, what it does, and stays in its lane.

Is it overkill for a personal setup? Maybe. But the conversations are better, the context is cleaner, and I never have to explain to my stock analyst why it's suddenly looking at Python stack traces.

That's the army of claws. 🐾
