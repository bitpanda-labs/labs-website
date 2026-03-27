---
layout: experiment
title: "Talk to Your Portfolio: The New Bitpanda AI Skill is Live"
date: 2026-03-26
categories: ["AI & Machine Learning", "User Experience"]
excerpt: "We built a lightweight CLI skill that lets AI agents read your Bitpanda account. When the first version was too slow, we just asked Claude to fix the code, dropping response times from 10 minutes to under a minute."
action: "Learn more"
---

We built a tool that lets AI agents talk directly to your Bitpanda account. It worked, but the initial version was painfully slow. Instead of manually profiling the code, we just fed it right back into the AI and asked it to fix the bottlenecks. A few iterations later, response times plummeted from 10 minutes to under 60 seconds. Here’s how we built it.


# The Pivot from MCP to CLI


Out of the box, agents like OpenClaw, Manus, or Claude Code are smart, but they don't know your crypto balances. A few weeks ago, we tried solving this by shipping a Python-based MCP (Model Context Protocol) server. It worked, but the AI landscape shifts fast, and heavyweight protocol servers are already losing favor to traditional CLI tools. The math is simple. A bloated MCP server like GitHub's eats up roughly 55,000 tokens just to explain what tools it has. In plain English, that means half of the AI's short term memory is completely wiped out before it even attempts to answer your question.
The industry is clearly moving toward lightweight CLI skills to save context, so we pivoted. We built a lightweight CLI wrapper for the Bitpanda Developer API that gives agents safe, read-only access to your portfolio, trade history, and asset prices.

# Optimizing the Code with Claude

Our first pass at the CLI was just a naive API wrapper. If you asked for your portfolio balance, it hit the wallets endpoint and then looked up every single asset and its price one by one, like waiting in a single-file checkout line. For a typical 30-asset portfolio, that meant making over 60 sequential API calls. Getting a simple total value took almost 10 minutes.
Instead of firing up a profiler, we pasted the code into Claude and asked why it was choking. It instantly flagged that single file bottleneck. It rewrote the logic to open up multiple lanes and fetch all the data at the exact same time. We ran it again, asked for another pass, and Claude pointed out we were fetching prices per-asset instead of in bulk. It restructured the code to pull all prices in a single request and cache the result locally.
A few iterations of this loop yielded concrete, usable optimisations. We skipped the benchmark suites and hours of debugging. By just having the AI review its own code, we dropped the execution time from 10 minutes down to about 30 seconds.

# Using It and What’s Next

While the engineering behind the scenes got pretty deep, the final product is built for everyone. You don't need to be a developer to actually use this. If you run an agent like OpenClaw, Cursor, or Manus, you just install the skill and talk to it normally. Ask it, "What’s my portfolio worth right now?" or "Show me my trades from last month," and the agent handles the API calls in the background.
It's live today on ClawHub and Skills.sh. The Agent Skills ecosystem is exploding right now. It hit over 350,000 packages faster than npm did, and we wanted Bitpanda integrated from day one.
A lot of our Bitpanda Labs projects are just internal prototypes, but we are actually shipping this one. If you have an account and use an AI agent, you can grab the package today. Honestly, the biggest takeaway for us wasn't just the integration, but the development pattern. Getting a massive speedup by having an AI iteratively fix its own code took less time than writing a Jira ticket. We're looking into adding more capabilities later, but for now, just go install it and try it out.
