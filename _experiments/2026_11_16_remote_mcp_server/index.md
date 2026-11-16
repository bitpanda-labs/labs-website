---
layout: experiment
title: "From Localhost to Production: Our Remote MCP Server"
date: 2026-11-16
categories: ["AI & Machine Learning", "API Development", "Developer Experience"]
excerpt: "Our first MCP server ran on your own computer and could only look at your portfolio. Eight months later, it runs on our side, it can trade behind server-side guardrails, and in ChatGPT all it takes is a sign-in."
action: "Learn more"
---

# Where it started

In February, we shipped [our first MCP server]({{ site.baseurl }}/experiments/2026_02_13_bitpanda_mcp/index/). It was a small prototype that let an AI assistant read your Bitpanda wallet, so you could ask Claude what your portfolio was worth and get a real answer. It proved the point quickly: connecting an existing API to an AI assistant can be simple.

Getting it running was another matter. The server lived on your own computer, which meant downloading code, setting up a developer environment and keeping a process running in the background. Many curious people never got as far as their first question. The ones who did soon wanted more than a view of their portfolio. They wanted to act on it, and the prototype could only look.

# A detour through the terminal

A month later, we tried a different route: a [lightweight CLI skill]({{ site.baseurl }}/experiments/2026_03_26_bitpanda_skill/index/) for agents that work in a terminal. It's still a great fit there. But most people don't talk to AI in a terminal. They use chat apps like ChatGPT and Claude, and those apps connect to outside services through MCP.

So we went back to MCP, this time with the prototype's lessons in hand. There should be nothing to install, and the assistant should be able to do something with your portfolio, not only look at it.

# One line instead of a setup guide

The server now runs on our side. You create an API key in your Bitpanda account, decide what it's allowed to do, and hand it to your assistant. In Claude Code, that's one command:

```bash
claude mcp add --transport http --scope user bitpanda https://mcp.public.bitpanda.com \
  --header "x-api-key: YOUR_BITPANDA_API_KEY"
```

There's nothing to keep up to date, either. When we ship a new tool, your assistant picks it up the next time it connects.

There are two servers: one for your Bitpanda account, covering prices, your portfolio, trading and staking, and one for [Fusion](https://www.bitpanda.com/en/fusion/mcp){:target="_blank"}, our platform for advanced traders. The [MCP server guide](https://docs.bitpanda.com/mcp-server-371916m0){:target="_blank"} walks through the setup for each client.

# Letting an assistant trade

Reading a portfolio is low-stakes. Trading isn't, and that's where most of our time went. One principle guided all of it: the safety net has to live on our servers, not in the assistant's instructions. A prompt can be ignored, misunderstood or hijacked by text the assistant picks up elsewhere. A rule that Bitpanda enforces can't.

So every trade takes two steps. The assistant first asks for a quote with the price and fee, and accepts it in a separate step. Nothing executes before that. Each key can only do what you allowed when you created it, and no clever wording changes that. You can also give a key a daily trading limit, in plain language:

```text
Set my daily trading limits to 100 EUR for buys and 100 EUR for sells.
```

Bitpanda enforces the limit on the key itself and rejects any trade that would go over it, even if the assistant goes off script. The limit is also permanent: once set, it can't be raised, lowered or removed, so for a different limit you create a new key. We recommend setting one before you give any key to an agent.

# A read-only door for ChatGPT

All of this assumes you're happy to create a key and paste a command. Many people aren't, and ChatGPT offers them a simpler way in: a plugin directory where you add a service in a few clicks. Being listed there means playing by ChatGPT's rules, though. A plugin can't execute trades or move money, and it can't ask people for credentials such as an API key.

So we opened a second endpoint, `mcp.public.bitpanda.com/read`. It's the same server with every tool that can move money taken out, and you sign in with your Bitpanda account instead of creating a key. That's what the [Bitpanda plugin](https://www.bitpanda.com/en/ai-investing-assistant){:target="_blank"} for ChatGPT runs on: search for Bitpanda in the plugin directory, sign in, and ask about your balances, holdings, performance or live prices. To let an assistant trade, you still connect the full server with a key.

ChatGPT is the first directory we're in, but it won't be the last. We're now working on connectors for other assistants, including Claude and Grok, so people can find Bitpanda wherever they already talk to AI.

# What we'd tell ourselves in February

The protocol was the easy part. The prototype came together quickly, and the eight months since went into trust: who is calling, what each key is allowed to do, and how much it can trade in a day. If you're building an MCP server for anything that touches money, start there.

The Bitpanda MCP is not an advisor, and its outputs are not investment advice. It retrieves information and carries out the actions your agent requests, within the scopes and limits of your API key. You remain responsible for reviewing and authorising every instruction.

Both servers are live, and so is the ChatGPT plugin. The [MCP server guide](https://docs.bitpanda.com/mcp-server-371916m0){:target="_blank"} covers setup for Claude Code, Claude Desktop, Cursor and Codex, and the [CLI](https://github.com/bitpanda-labs/bitpanda-cli){:target="_blank"} is open source.
