---
title: "Terminal-Bench 2.0: same model, 24-point gap — the harness is everything"
date: 2026-04-02
category: ai
source: neuro
---

[Terminal-Bench 2.0](https://www.tbench.ai/leaderboard/terminal-bench/2.0) is a benchmark for terminal-based coding agents — real tasks in real terminals, not just patch generation. What makes it fascinating is filtering the [leaderboard](https://www.tbench.ai/leaderboard/terminal-bench/2.0) by a single model.

When you look at agents all running **Claude Opus 4.6**, the spread is enormous:

| Agent | Accuracy |
|-------|----------|
| ForgeCode | 81.8% |
| Capy | 75.3% |
| Terminus-KIRA | 74.7% |
| Junie CLI (JetBrains) | 71.0% |
| Droid (Factory) | 69.9% |
| Claude Code (Anthropic) | 58.0% |

That's a **~24 point gap** between the best third-party harness and Anthropic's own Claude Code — using *the exact same model*. The irony: Anthropic builds the best model but ships one of the weakest harnesses for it.

This is the strongest quantitative evidence yet for the "harness > model" thesis. The model is a commodity; the scaffolding around it — tool design, context management, edit formats, retry logic, task decomposition — determines whether it performs at 58% or 82%.

Three independent data points now converge on this:

1. [The Hashline edit format](https://blog.can.ac/2026/02/12/the-harness-problem/) showed that changing how the model references code lines dramatically reduces edit failures
2. [OpenAI's Harness Engineering post](https://openai.com/index/harness-engineering/) showed that agent environment design matters more than model capability
3. Terminal-Bench 2.0 puts hard numbers on the gap: same model, vastly different results

If you're building with AI agents, stop chasing the next model release. Invest in your harness.
