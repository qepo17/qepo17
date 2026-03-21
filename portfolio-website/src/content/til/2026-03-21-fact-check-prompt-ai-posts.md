---
title: "A structured prompt for fact-checking and detecting AI-generated posts"
date: 2026-03-21
category: ai
source: neuro
---

Found a well-structured prompt template for auditing social media posts — especially geopolitical content that may be AI-generated. It runs two separate analyses:

**Analysis 1: Line-Item Fact Check**
Extract every specific factual claim (military units, ship names, troop numbers, casualty figures, oil prices, named officials, institutional attributions) and verify each against at least two independent sources. Verdicts:
- **CONFIRMED** — verified by multiple sources
- **DISTORTED** — approximately correct but misstated in degree/detail
- **UNVERIFIED** — no source found for the specific claim as written
- **FRAMING** — interpretation presented as declarative fact (e.g. "The call was not about peace. It was about oil.")

**Analysis 2: AI Generation Probability**
Score 6 indicators from 0–10:
1. **Structural uniformity** — follows high-volume AI content templates (dramatic opener → hardware inventory → price data → strategic interpretation → literary closer)
2. **Sourcing absence** — interpretive claims without named analysts
3. **Cinematic detail** — narrative flourishes connecting unrelated facts through implied causation
4. **Hedge avoidance** — unqualified declarative language without uncertainty markers
5. **Volume consistency** — posting frequency vs. realistic human research timelines
6. **Comment engagement** — does the author engage with factual challenges, or post and move on?

**Takeaway:** The "FRAMING" verdict category is particularly useful. A lot of viral geopolitical content mixes real facts with interpretive leaps stated as though they're facts too. Splitting these apart is the key skill — both for humans reading the post and for AI auditing it.

Credit: [@TheBlackWallaby](https://x.com/TheBlackWallaby/status/2035332203010474386)
