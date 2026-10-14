---
layout: experiment
title: "From Localhost to Production: Our Remote MCP Server"
date: 2026-10-14
categories: ["AI & Machine Learning", "API Development", "Developer Experience"]
excerpt: "Our first MCP server was a local prototype that required cloning and manual setup. Eight months later, it has become a hosted service at mcp.public.bitpanda.com. Most clients connect with nothing but a scoped API key, and server-side guardrails keep trading agents in check."
action: "Learn more"
---

# Where we started

In February, we released [an MCP server for the Bitpanda Developer API]({{ site.baseurl }}/experiments/2026_02_13_bitpanda_mcp/index/). It was a lightweight FastAPI wrapper built with `fastmcp`, and it let AI assistants read wallet balances and transaction history. It showed that connecting an existing API to an AI agent can be simple.

Running it locally was another story. Users had to clone the repository, install Python 3.11 and Poetry, fill in a `.env` file, start the server and point their client at `http://localhost:8000/mcp` with their API key in a header. Every one of those steps cost us users. The server was also read-only: you could look at your portfolio, but you couldn't act on it.

Here's how we changed the architecture, and what we learned along the way.

# What the prototype taught us

Feedback from early users came down to three things.

**Installation was holding adoption back.** The tools worked well, but setting them up was too much for people who just wanted to talk to their portfolio. Running locally also meant managing your own Python environment and running `git pull` to get updates.

**Read-only wasn't enough.** Checking balances is useful, but users quickly wanted to trade straight from the assistant, and a read-only server can't do that.

**Guardrails have to live on the server.** Once an agent can place orders, the rules need to be enforced server-side. Instructions in a system prompt aren't enough on their own, especially once prompt injection is in play.

# What we built instead

The server is now hosted, and it uses the HTTP transport:

| Server | Endpoint |
|---|---|
| Public API | `https://mcp.public.bitpanda.com` |
| Fusion | `https://mcp.fusion.bitpanda.com` |

The two endpoints serve different needs. The Public API server works with your Bitpanda account: portfolio, prices, trading and Earn. [Fusion](https://www.bitpanda.com/en/fusion/mcp){:target="_blank"} is built for advanced traders and aggregates liquidity across 12+ venues. Both take the same `x-api-key` header, but each needs its own key: Fusion keys are created in a separate Fusion tab.

Connecting takes one command:

```bash
claude mcp add --transport http --scope user bitpanda https://mcp.public.bitpanda.com \
  --header "x-api-key: YOUR_BITPANDA_API_KEY"
```

For most clients, there's no local installation and no background process. Create an API key in [Account Settings](https://app.bitpanda.com/my-account/apikey){:target="_blank"}, pick its permissions and pass it in the request header. That works in any MCP client that can send a custom header, including Claude Code, Codex, Cursor and Claude Desktop (through a small local bridge). The [MCP server guide](https://docs.bitpanda.com/mcp-server-371916m0){:target="_blank"} has setup steps for each.

Fifteen tools ship on the Public API server today, covering market data, portfolio state, Earn staking, trading and trading limits. The Fusion server has twelve of its own for market data, its order book, orders and account.

# Designing for an agent that can trade

Read tools are forgiving. Write tools aren't, and that's where most of the engineering on the Public API server went.

**Trades take two steps.** The agent first requests a quote, which includes the price and fee, then accepts it in a separate call. The price is known before anything executes, and a single malformed tool call can't execute a trade nobody meant to make.

**Scopes are set when the key is created.** Trade and Earn each have separate read and write scopes, alongside Balances and Transaction. A key you give an experimental agent can be read-only, and no amount of clever prompting expands it. Keys are valid for at most a year and can be restricted to specific IP addresses.

**Rate limits are enforced on the server.** Each user can make 50 read requests, 5 trade requests and 5 staking requests per second, so a runaway retry loop hits a wall instead of your account.

**Daily trading limits come with their own tools.** You can set a daily cap in plain language:

```text
Set a trading limit of 100 EUR per day for both buy and sell.
```

The server enforces the limit per API key, so it doesn't depend on the agent behaving. It still holds if the agent is hit by a prompt injection. Two design choices matter here:

- **Limits are permanent once set.** They can't be changed or removed. To use a different limit, you create a new API key.
- **Buy and sell are set separately.** The tool needs a buy limit, a sell limit and a currency, so if you only give one number, your assistant should ask whether it applies to both instead of guessing with your money.

Daily limits are optional, but we recommend setting one by default.

# What we'd tell the version of us that started this

The protocol was the easy part. The `fastmcp` prototype came together quickly, and moving to a hosted server needed few protocol changes. Most of the work went into what production needs:

- authentication with scoped API keys;
- per-key budgets and server-side rate limits;
- scope isolation, and a separate quote and accept step for every trade.

The Bitpanda MCP is not an advisor, and its outputs are not investment advice. It retrieves information and carries out the actions your agent requests, within the scopes and limits of your API key. You remain responsible for reviewing and authorising every instruction.

Both servers are live. The [MCP server guide](https://docs.bitpanda.com/mcp-server-371916m0){:target="_blank"} covers setup for Claude Code, Claude Desktop, Cursor and Codex, and the [CLI](https://github.com/bitpanda-labs/bitpanda-cli){:target="_blank"} is open source.
