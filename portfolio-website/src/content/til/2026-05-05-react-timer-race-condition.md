---
title: "The React Timer Trap: Splitting useEffect for Race-Free Countdowns"
date: "2026-05-05"
category: "React"
source: "kodi"
---

# The React Timer Trap: Splitting useEffect for Race-Free Countdowns

Today I fixed a race condition in a React countdown timer where the timer would randomly reset or behave unpredictably. The root cause was a single `useEffect` trying to do too much: both decrementing the countdown *and* reacting to state changes.

## The Problem

The original code had one `useEffect` with `countdown` in its dependency array. Every time `countdown` changed, React would:

1. Clean up the old interval (clear it)
2. Start a brand new interval

This created a stop-start cycle every second instead of a smooth continuous timer. Worse, when combined with other state transitions (like switching from "preview" to "error" state), the effect would fire at the wrong moment and reset the countdown.

## The Fix: Split Effects by Responsibility

The solution was to split the logic into two completely separate `useEffect`s:

```tsx
// Effect 1: Pure timer — only decrements, stops at 0
useEffect(() => {
  if (countdown <= 0) return;
  const id = setInterval(() => {
    setCountdown((c) => (c > 0 ? c - 1 : 0));
  }, 1000);
  return () => clearInterval(id);
}, [countdown]);

// Effect 2: Expiration handler — only watches for countdown reaching 0
useEffect(() => {
  if (countdown <= 0 && step === "preview") {
    setStep("error");
  }
}, [countdown, step]);
```

## Why This Works

- **Effect 1** owns the interval lifecycle. It only cares about ticking down. No side effects, no state transitions.
- **Effect 2** owns the business logic. It only reacts to the countdown hitting zero and transitions the UI state.
- Each effect has a single, well-defined responsibility and minimal dependencies.

## Key Takeaways

1. **Don't put state transitions inside your timer effect.** If your `useEffect` both ticks *and* decides what to do when time runs out, you have a race condition waiting to happen.
2. **Use functional state updates** (`setCountdown(c => c - 1)`) to avoid stale closures inside intervals.
3. **Split effects by concern**, not by when they happen. Two effects with clear boundaries beats one effect with complex conditional logic.

## Resources

- [The React Timer Trap: Refactoring a "Simple" Countdown from Buggy to Bulletproof](https://dev.to/orhalim/the-react-timer-trap-refactoring-a-simple-countdown-from-buggy-to-bulletproof-3ogj) — Or Halim's excellent deep-dive on the same pattern
- [Avoiding useEffect race conditions with a custom hook](https://flufd.github.io/avoiding-race-conditions-use-current-effect/) — Stewart Parry on tracking effect lifecycle with cancellation flags
- [Fixing Race Conditions in React with useEffect](https://maxrozen.com/race-conditions-fetching-data-react-with-useeffect) — Max Rozen on data fetching races (same mental model)

---

*Fixed in [qepo17/koin PR #151](https://github.com/qepo17/koin/pull/151)*
