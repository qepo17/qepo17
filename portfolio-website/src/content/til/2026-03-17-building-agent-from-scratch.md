---
title: "Building an AI Agent from Scratch in Rust"
date: 2026-03-17
category: ai
source: kodi
---

Built a minimal AI agent in ~150 lines of Rust to understand the core agent loop pattern. The repo: [qepo17/kodi](https://github.com/qepo17/kodi).

**The agent loop is surprisingly simple:**

1. Send messages to an LLM (via OpenRouter → Claude Sonnet)
2. Check if the response contains tool calls
3. If yes → execute the tools, append results to message history, loop back to step 1
4. If no → print the final response and exit

**The architecture** breaks down into four clean files:

- `models.rs` — OpenAI-compatible request/response types (serde-powered)
- `clients.rs` — The agent loop: send → check tool calls → execute → repeat
- `tools.rs` — Tool execution (currently just `bash`)
- `main.rs` — Entry point, reads API key and query from args

**Key implementation details:**

- Messages use a tagged enum (`#[serde(tag = "role")]`) for clean serialization of system/user/assistant/tool messages
- Tool call results get appended as `tool` role messages with the matching `tool_call_id`
- The `finish_reason` field distinguishes between `"tool_calls"` (keep looping) and `"stop"` (done)
- Tool arguments come as a JSON string, not a parsed object — you parse them yourself

**The takeaway:** the "magic" of AI agents is just a while loop with tool dispatch. The model decides *what* to do, the harness decides *how* to do it. Once you see the pattern, the whole concept clicks — and you realize you can build one yourself without any framework.
