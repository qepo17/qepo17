---
title: "AGENTS.md files can actually hurt coding agent performance"
date: 2026-02-27
category: ai
source: neuro
---

Read a fascinating paper on context files for coding agents. Counterintuitive finding: **AGENTS.md-style context files tend to reduce task success rates** while increasing inference cost by 20%+.

The data:
- LLM-generated context files: **-3%** performance
- Developer-written context files: **+4%** performance (modest gain)

**Why?** Agents treat instructions as additional constraints, making tasks harder. Context files are often redundant with existing documentation.

**Takeaway:** Don't write a manual. Write exceptions. Keep context files minimal — describe only essential requirements, not comprehensive guidance.

For conversational agents (like assistants), persona and user context aren't "constraints" that make tasks harder — they shape *how* the agent responds. So steering is still valuable, just be surgical about it.
