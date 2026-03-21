---
title: "Karpathy's Autoresearch Pattern — Applied to Trading Strategy Discovery"
date: 2026-03-21
category: ai
source: kodi
---

Andrej Karpathy's [autoresearch](https://github.com/karpathy/autoresearch) pattern — where an AI agent autonomously edits code, runs experiments, and keeps improvements — has been adapted for trading strategy discovery by [Nunchi-trade/auto-researchtrading](https://github.com/Nunchi-trade/auto-researchtrading).

## The Pattern

The architecture is elegantly simple: **one mutable file, everything else locked down**.

1. `strategy.py` — the *only* file the AI agent can modify
2. `prepare.py` — data fetching + backtest engine (immutable)
3. `backtest.py` — runner that scores each iteration (immutable)
4. `program.md` — instructions that tell the AI to loop: edit → commit → backtest → keep if better, revert if worse

The AI agent (Claude Code) autonomously modifies the strategy, backtests against historical data, and repeats. No human in the loop.

## Results & Skepticism

They claim 103 autonomous experiments took a simple momentum strategy from Sharpe 2.7 → 21.4 with 0.3% max drawdown. The final strategy is a 6-signal ensemble (momentum, EMA crossover, RSI, MACD, Bollinger Band compression) with majority vote.

**Interesting finding:** most gains came from *removing* complexity, not adding it.

But a Sharpe of 21.4 on crypto with 0.3% max drawdown screams overfitting. No out-of-sample or forward test results, and the fee model (2-5 bps + 1 bps slippage) is optimistic for real execution at scale.

## The Takeaway

The *pattern* is the real value here, not the specific trading results. "Single mutable file + automated scoring loop" is a powerful constraint that works for any domain with a measurable objective — ML training (Karpathy's original), trading strategies, prompt engineering, even config tuning.

The key design choice: make the search space narrow (one file) and the evaluation honest (out-of-sample validation). Without that second part, you're just building an overfitting machine.
