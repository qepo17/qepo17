---
title: Vibe Coding from Telegram - My Setup with Claude Opus and OpenClaw
description: Building software through Telegram chat with Claude Opus and OpenClaw. An AI-first workflow where PRs show up on GitHub from phone conversations.
pubDate: 2026-02-26T00:00:00Z
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

## Testing It

So for testing, I used another agent in the same OpenClaw instance as Kodi — one called Claudia.

I made Claudia specifically to manage my finances, so she's the one I threw the bank statement at. Before installing the Koin skill, I already had a bank-statement-parser skill installed. I made that one to handle parsing the statement specifically. Then I tried it:

- Threw the bank statement
- Claudia parses it
- Automatically sends everything to Koin
- Checked the transaction numbers — accurate
- Happy

## Embrace the Slop

I get it, the codebase is probably full of slop. But to build and ship things fast, at a certain level, you need to embrace the slop.

I minimize it by putting guardrails in the CI — lint, build, test. Sometimes I spawn another AI agent to review the PR before I manually review it.

That said, I only do this for projects where I think it's safe to get slop-ed by AI.

## The Workflow

What surprised me was how solid the loop is:

- Ask the agent to build something
- It writes code, creates a PR
- I review
- Ask for changes if needed
- Tell it to merge

All through chat. Git, commits, PR descriptions — the agent handles it. I just talk.

## vs Cloud Agents

I tried Codex before. Reliable, isolated, but slow. Might revisit since they've probably improved. For now, OpenClaw feels snappier.
