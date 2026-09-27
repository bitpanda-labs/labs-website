---
layout: experiment
title: "From Localhost to Production: Our Remote MCP Server"
date: 2026-09-01
categories: ["AI & Machine Learning", "API Development", "Developer Experience"]
excerpt: "Our first MCP server was a local prototype you had to clone and run yourself. Eighteen months later it's a hosted service at mcp.public.bitpanda.com — no install, eleven tools, and server-side guardrails for agents that can trade."
action: "Learn more"
---

# Where we started

In February we published [an MCP server for the Bitpanda Developer API]({{ site.baseurl }}/experiments/2026_02_13_bitpanda_mcp/index/) — a thin FastAPI wrapper built with `fastmcp` that exposed wallet balances and transaction history as tools an AI assistant could call. It worked, and it made a point we still believe: the gap between "I have an API" and "my agent can use my API" is much smaller than it looks.

But it ran on your machine. To use it you cloned a repo, installed Python 3.11, managed a virtualenv, exported an API key, and pointed your client at `http://localhost:8000/mcp`. Every one of those steps was a place to lose someone. And it was read-only — it could tell you what you owned, but never act on it.

This post is about what we changed, and what we learned changing it.

# What the prototype taught us

Three things surfaced quickly once people outside the team started using it.

**Installation was the product's ceiling.** The tools were easy; getting to them wasn't. Most of the people who wanted conversational access to their portfolio were not going to debug a virtualenv to get it. A local server means every user runs their own copy, on their own Python version, with their own upgrade schedule — and when we shipped a fix, nobody got it until they remembered to `git pull`.

**Read-only is a demo, not a workflow.** "What's my Bitcoin balance?" is a nice first conversation. The second question is always some variant of "then buy me some more," and that's where a read-only server stops being interesting.

**Trust has to live server-side.** The moment you consider letting an agent place an order, every guardrail you were happy to leave to convention becomes something the server has to enforce. An agent that's been prompt-injected won't respect a limit that only exists in its own instructions.

# What we built instead

The server is now hosted. There's nothing to install, and it speaks HTTP transport:

| Server | Endpoint |
|---|---|
| Public API | `https://mcp.public.bitpanda.com` |
| Fusion | `https://mcp.fusion.bitpanda.com` |

Two endpoints because they're two different products — the Public API for broker accounts across crypto, stocks, ETFs and metals, and [Fusion](https://www.bitpanda.com/en/fusion/mcp){:target="_blank"} for professional traders working against aggregated liquidity from 12+ venues. Same protocol, same auth model, different execution engine behind them.

Connecting is one command:

```bash
claude mcp add --transport http --scope user bitpanda https://mcp.public.bitpanda.com \
  --header "x-api-key: YOUR_BITPANDA_API_KEY"
```

That's the whole setup. No clone, no runtime, no local process to keep alive. You generate a key in [Account Settings](https://app.bitpanda.com/settings/api){:target="_blank"}, assign it scopes, and pass it as a header. The same works for Claude Desktop, Cursor, the ChatGPT app and Codex CLI — the [documentation](https://docs.public.bitpanda.com){:target="_blank"} has the config for each.

Eleven tools ship today, spanning market data, portfolio state, Earn staking actions, and order execution.

# Designing for an agent that can trade

Read tools are forgiving. Write tools are not, and most of the engineering went here.

**Orders are two-step.** Placing a trade is a request-for-quote flow: the agent requests a quote, then accepts it as a separate call. The quote is a concrete, priced thing a human can look at before it becomes an order. It also means an agent can't stumble into a trade with one malformed tool call — it has to deliberately take the second step.

**Scopes are set at key creation.** Read, Trade and Earn are separate. A key you hand to an experimental agent can be read-only, and no amount of clever prompting expands it. Keys expire within a year and can be pinned to specific IPs.

**Rate limits are per-key and server-side** — 20 requests/second for reads, 5 for trading and staking. Agents retry loops in ways humans don't; this is the backstop.

**Daily trading limits are themselves a tool.** This is the piece we're happiest with. You can ask your assistant, in plain language:

```text
Set a trading limit of 100 EUR per day for both buy and sell.
```

The cap is enforced per API key, per day, by the server — not by the agent. It holds even if the agent misbehaves or gets prompt-injected, because the component being asked to respect the limit isn't the component that could be compromised. Two deliberate design choices came out of that:

- Limits are **permanent for the life of the key**. Once set, they can't be raised or removed — calling the tool again on a limited key returns a conflict. To change a limit, you create a new key. An agent that can talk its way into a higher ceiling isn't a ceiling.
- The set tool requires **separate buy and sell values**. If you give it one number, a well-behaved assistant should ask which side you meant rather than guessing. Ambiguity in a financial tool signature is a bug.

Setting a limit is optional. For any key you hand to an agent, we'd treat it as the default.

# What we'd tell the version of us that started this

The protocol was never the hard part. `fastmcp` made the February prototype almost trivial, and the hosted rewrite didn't get meaningfully harder at the MCP layer. What took the time was everything a local read-only prototype lets you defer: authentication you can hand to a stranger, per-key budgets, rate limits, scope boundaries, and a confirmation step that survives contact with an unreliable caller.

That's the actual lesson from eighteen months of this. Exposing an API to an agent is a weekend. Making it safe to hand an agent your account is the work — and almost all of that work is server-side, because the agent is precisely the thing you can't trust to enforce its own limits.

The Bitpanda MCP is not an advisor and its outputs are not investment advice. It retrieves information, prepares actions, and executes only what you confirm — you remain responsible for reviewing and authorising every instruction.

Both servers are live. The [documentation](https://docs.public.bitpanda.com){:target="_blank"} has setup for every major client, and the [CLI is open source](https://github.com/bitpanda-labs/bitpanda-mcp){:target="_blank"}.
