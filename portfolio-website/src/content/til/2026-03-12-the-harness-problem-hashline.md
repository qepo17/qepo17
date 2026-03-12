---
title: "The Harness Problem: most coding agent failures are edit format failures, not model failures"
date: 2026-03-12
category: ai
source: neuro
---

Can Bölük's blog post [The Harness Problem](https://blog.can.ac/2026/02/12/the-harness-problem/) makes a compelling case: when an AI coding agent fails to edit a file correctly, we blame the model — but the real culprit is usually the **edit format** itself.

Current edit formats like `apply_patch` (OpenAI's diff-style format) and `str_replace` demand perfect text reproduction. The model has to reproduce the exact lines it wants to change, character-for-character. When it can't — and it often can't — the edit silently fails or corrupts the file.

**The Hashline solution:** instead of reproducing content, reference lines by a short content hash. The model says *which* lines to change using stable identifiers, not fragile text matching. No reproduction needed.

The results across 15 LLMs are striking:

- **Grok Code Fast 1**: 6.7% → 68.3% (a 10× improvement)
- **MiniMax**: 2×+ improvement
- **Grok 4 Fast**: same accuracy, 61% fewer tokens
- **Gemini models**: gains equivalent to a full model upgrade

The weakest models benefited the most — their "coding ability" was being completely hidden behind mechanical edit failures. But even top models gained meaningfully.

**The key insight:** a harness upgrade is free, takes an afternoon, and can deliver gains equivalent to (or better than) an expensive model upgrade that takes months. The bottleneck isn't the model's intelligence — it's the interface between intent and execution. This builds directly on the [harness engineering](https://openai.com/index/harness-engineering/) trend from earlier this month, but with hard numbers proving the point.
