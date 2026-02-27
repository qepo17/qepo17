---
title: 'Vibe Coding from Telegram: It Actually Works'
description: Building software through Telegram chat with Claude Opus and OpenClaw. An AI-first workflow where PRs show up on GitHub from phone conversations.
pubDate: 2026-02-27T00:00:00Z
heroImage: '/assets/images/posts/openclaw.png'
tags: ['ai', 'coding', 'workflow', 'openclaw']
---

So lately I've been building software from Telegram. Just chatting with an AI on my phone, and PRs show up on GitHub.

The stack is Claude Opus running through OpenClaw. I've got a coding agent named Kodi that handles the dev work.

## What I Built

An income-expense tracker, API and AI-first.

This has been a long problem for me — it's hard to consistently input your income and expenses manually. I did it for several months: when I got my salary, I'd input it manually, but then the numbers were different. That's another headache to reconcile everything. I'm trying to solve that with this personal vibe-coding project.

Here's how it works:

1. Install a SKILL.md (instructions for the agent)
2. Hand it a bank statement PDF
3. Agent parses it, hits the Koin API to log transactions
4. PDF gets deleted after processing

The data stays in Koin. No files hanging around.

## The Workflow

![Kodi](/assets/images/posts/kodi-koin.png)
![Kodi](/assets/images/posts/kodi-koin-2.png)

So what does building look like?

- Ask the agent to build or fix something
- It writes code, creates a PR
- I review
- Ask for changes if needed
- Tell it to merge

All through chat. Git, commits, PR descriptions — the agent handles it. I just talk. I'm a bit surprised at how seamless the flow is. The agent is very smart and can follow instructions perfectly. I rarely got an error or found a case where he misbehaved outside of instructions. I told him to do something, and he can do it. 

🧑 "I have an idea to build [a product], please create a plan for it."

🤖 "[Here's the plan]"

🧑 "I think [you should revise this-and-that part]."

🤖 "[Updated the plan]"

🧑 "Ok, looks cool now. Split the feature into smaller tasks. You can create GitHub issues for it."

🤖 "[10 issues created]. Want to do #1 next?"

🧑 "Let's do it."

That's it. Honestly, I'm impressed.

## Testing It

![Claudia](/assets/images/posts/claudia-koin.png)

So for testing, I used another agent in the same OpenClaw instance as Kodi — one called Claudia.

I made Claudia specifically to manage my finances, so she's the one I threw the bank statement at. Before installing the Koin skill, I already had a bank-statement-parser skill installed. I made that one to handle parsing the statement specifically. Then I tried it:

- Threw the bank statement
- Claudia parses it
- Automatically sends everything to Koin
- Checked the transaction numbers — accurate
- Happy

## Embrace the Slop

Look, I get it, the codebase is probably full of slop. But to build and ship things fast, at a certain level, you need to embrace the slop.

I minimize it by putting guardrails in the CI — lint, build, test. Sometimes I spawn another AI agent to review the PR before I manually review it.

That said, I only do this for projects where I think it's safe to get slopped by AI. I understand the concerns of OpenClaw and full-vibecoding, hence I am not doing it (yet) for work-related projects.

## vs Cloud Agents

I tried Codex back in August 2025. Reliable and well-sandboxed, but slow as hell. The biggest issue was the feedback loop — you submit a task, wait, and hope it got things right. If it didn't, you submit again. There's no real conversation.

With OpenClaw, I'm talking to the agent in real-time. I can course-correct mid-task, give extra context, or tell it to stop and try a different approach. That back-and-forth is what makes it feel like pair programming rather than a ticket queue.

Cloud agents probably have their place for well-defined, isolated tasks. Codex might have improved since I last tried it, I might need to revisit at some point. But for now, I'm happy with OpenClaw. It's surprisingly seamless to just talk to an agent and watch code ship.
