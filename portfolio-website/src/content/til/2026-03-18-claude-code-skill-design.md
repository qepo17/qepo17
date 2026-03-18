---
title: "9 Skill Categories for Claude Code (from Anthropic's Internal Use)"
date: 2026-03-18
category: ai
source: kodi
---

Thariq's article reveals how Anthropic internally categorizes Claude Code skills into 9 types — plus practical tips for writing effective ones.

## The 9 Skill Categories

| # | Category | Examples |
|---|---|---|
| 1 | Library/Tool Reference | billing-lib, internal-cli, frontend-design |
| 2 | Verification | signup-flow-driver, checkout-verifier |
| 3 | Data & Monitoring | funnel-query, grafana, cohort-compare |
| 4 | Workflow Automation | standup-post, create-ticket, weekly-recap |
| 5 | Scaffolding | new-workflow, new-migration, create-app |
| 6 | Code Quality | adversarial-review, code-style, testing-practices |
| 7 | Deployment | babysit-pr, deploy-service, cherry-pick-prod |
| 8 | Debugging/Incident | oncall-runner, log-correlator, service-debugging |
| 9 | Maintenance/Ops | resource-orphans, dependency-management, cost-investigation |

## Key Tips for Writing Skills

1. **Skills are folders, not just markdown** — Include scripts, assets, reference code, templates
2. **Gotchas section = highest signal** — Built from real Claude failure points
3. **Progressive disclosure** — Split into `references/`, `assets/`, etc. Tell Claude what's there
4. **Be flexible, not rigid** — Give info but let Claude adapt
5. **Description = trigger condition** — Not a summary, but "when to use this"
6. **Config.json for user setup** — Store per-user settings like Slack channels
7. **Skills can have memory** — Log files, JSON, SQLite. Use `${CLAUDE_PLUGIN_DATA}` for stable storage
8. **Give Claude scripts** — Code > prose for repetitive tasks
9. **Skill-scoped hooks** — e.g. `/careful` blocks dangerous commands only when activated

**The takeaway:** Skills aren't just documentation — they're mini-applications. The best ones combine markdown guidance with executable scripts, persistent memory, and smart trigger conditions. Think of them as giving Claude a specialized toolkit, not just instructions.
