---
title: "OpenAI Symphony: a masterclass in writing specs for AI agents"
date: 2026-03-05
category: ai
source: neuro
---

OpenAI open-sourced [Symphony](https://github.com/openai/symphony) — a service that polls Linear issues, creates isolated workspaces, and runs Codex agents to do the work. But the real gem isn't the code — it's the [SPEC.md](https://github.com/openai/symphony/blob/main/SPEC.md). It's a near-perfect example of how to write specs that AI agents can execute against.

**Why this spec is good to throw at an AI agent:**

- **6-layer architecture** clearly separated: policy, config, coordination, execution, integration, observability
- **WORKFLOW.md as in-repo config** — the agent prompt and runtime settings are version-controlled with the code, not buried in a dashboard
- **Lifecycle hooks** (`after_create`, `before_run`, `after_run`, `before_remove`) give agents clear extension points without touching core logic
- **No database required** — recovers from tracker + filesystem state alone, which makes it simple for agents to reason about
- **Hot reload config** without restart — designed for iteration speed
- **Clear non-goals** — explicitly states what it won't do, which prevents agents from overbuilding

**The pattern:** Symphony treats the agent as a worker bee. The spec defines the hive — poll cadence, concurrency bounds, workspace isolation, retry with backoff, observability. The agent just executes inside its sandbox.

**Key insight:** This IS [harness engineering](https://openai.com/index/harness-engineering/) made concrete. The blog post explained the philosophy; this spec shows the implementation blueprint. If you're building agent infrastructure, this is the reference to study.

**Limitations (draft v1):** Linear-only tracker, Codex-only agent, no sandbox mandate, no multi-tenant support. But the architecture is clean enough to extend.
