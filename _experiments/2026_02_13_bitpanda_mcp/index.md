---
layout: experiment
title: "Bridging APIs and AI: Our MCP Server for Bitpanda"
date: 2026-02-13
categories: ["AI & Machine Learning", "API Development"]
excerpt: "We built an MCP server that makes the Bitpanda Developer API accessible to AI agents. Now Claude and other AI assistants can access your wallet balances, transaction history, and account data through natural conversation."

action: "Learn more"
---

# The API Integration Challenge

APIs are everywhere. They're how systems talk to each other, how data flows between services, and how developers build on top of platforms. But here's the thing: traditional APIs require code, documentation reading, authentication setup, and careful request formatting.

What if AI agents could interact with APIs as naturally as they interact with users?

That's the question that led us to build an MCP server for the Bitpanda Developer API.

# What is MCP?

Model Context Protocol (MCP) is an open standard that enables AI assistants to connect with external data sources and tools. In practical terms, think of it like installing a plugin or extension for your AI assistant—like Claude or ChatGPT—giving it new capabilities it didn't have before.

Instead of AI assistants being limited to what they know from training, MCP lets them access real-time data from services you actually use. It's a universal adapter that any AI assistant can plug into.

# Our Solution: A Thin Wrapper with Big Impact

We built [bitpanda-public-api-mcp](https://github.com/bitpanda-labs/bitpanda-public-api-mcp){:target="_blank"}, an open-source MCP server that wraps the [Bitpanda Developer API](https://developers.bitpanda.com/){:target="_blank"}. It's built with FastAPI and uses [fastmcp](https://github.com/jlowin/fastmcp){:target="_blank"}, a dedicated tool that makes creating MCP servers surprisingly straightforward.

The server exposes the Bitpanda Developer API v1.1 as MCP tools, allowing AI agents to:
- Access your wallet balances across all assets
- Retrieve your transaction history with flexible filtering (by wallet, asset, date range, flow direction)
- Get asset information (name, symbol) by identifier
- Query personal account data through natural conversation

# What This Enables

The real magic isn't in the code, it's in what becomes possible:

**Natural Language Account Access**<br>
Once connected to an MCP-compatible AI assistant, you can simply ask: "What's my Bitcoin balance?" or "Show me all my transactions from last month" or "Which wallets had activity this week?" Instead of writing API calls, you’re having a normal conversation with your AI Assistant. The AI handles the API interaction automatically.

**Conversational Financial Management**<br>
Building personal finance tools, portfolio trackers, or transaction analyzers suddenly becomes a conversation. The friction between idea and implementation drops dramatically.

**Simplified Access**<br>
Non-technical users can now access their Bitpanda account data through AI assistants. You don't need to understand REST APIs, authentication headers, or JSON parsing. Just ask.

**AI-Native Development**<br>
This is part of a larger shift: tools that are built for AI from the ground up. Not just AI-assisted development, but AI as the primary interface.

# Building with fastmcp

One of the pleasant surprises was how straightforward the implementation was. Using [fastmcp](https://github.com/jlowin/fastmcp){:target="_blank"}, a Python framework specifically designed for building MCP servers, we could focus on exposing the API endpoints rather than wrestling with protocol implementation details.

The result is a clean, maintainable codebase that anyone can fork, extend, or adapt for their own needs.

# Ready to Use

The entire project is [available on GitHub](https://github.com/bitpanda-labs/bitpanda-public-api-mcp){:target="_blank"}. Whether you want to:
- Use it with Claude Code or other MCP-compatible AI assistants
- Learn how MCP servers work
- Build your own MCP integrations
- Extend the functionality

The code is there, ready to explore.
