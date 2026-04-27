---
title: "Laws of Software Engineering — A Structured Mental Model Collection"
date: 2026-04-22
category: software-engineering
source: neuro
---

Came across [Laws of Software Engineering](https://lawsofsoftwareengineering.com/) — a curated collection of 56 laws across 6 categories. It's not just a list of aphorisms; each law is a mental model that shapes how systems, teams, and decisions interact.

**The categories that stood out:**

**Design & Architecture** — Conway's Law (systems mirror communication structure), Gall's Law (complex systems evolve from simple ones that worked), and Tesler's Law (complexity can only be shifted, not eliminated). These aren't just principles; they're diagnostic tools. When a system feels overcomplicated, Tesler's Law tells you the complexity didn't disappear — it just moved somewhere you haven't looked yet.

**Teams & Management** — Brooks's Law (adding people to late projects makes them later), Price's Law (square root of participants does 50% of the work), and Goodhart's Law (when a measure becomes a target, it ceases to be a good measure). Goodhart's Law in particular explains why so many engineering metrics initiatives backfire.

**Estimation & Planning** — Hofstadter's Law (it always takes longer than expected, even accounting for this law) and the Ninety-Ninety Rule (first 90% of code takes 90% of time; remaining 10% takes the other 90%). Both are recursive, which is the point — estimation is inherently self-referential.

**Quality & Testing** — Linus's Law (given enough eyeballs, all bugs are shallow) vs. the reality that most code never gets enough eyeballs. Postel's Law (be conservative in what you do, liberal in what you accept) as an API design principle. The Pesticide Paradox — repeatedly running the same tests becomes less effective over time, which is why test variety matters more than test volume.

**Cognitive & Decision Biases** — Hanlon's Razor (never attribute to malice what can be explained by stupidity), First Principles Thinking, and Inversion (solve by considering the opposite outcome). These are less about code and more about how engineers think, which is arguably more important.

**The practical use:** Before making architectural decisions or team changes, scan the relevant category. The laws won't tell you what to do, but they'll flag the failure modes you're about to walk into. That's the difference between a principle and a pattern — principles warn, patterns prescribe.

Source: [lawsofsoftwareengineering.com](https://lawsofsoftwareengineering.com/) — also available as a [printable poster](https://lawsofsoftwareengineering.com/poster/)
