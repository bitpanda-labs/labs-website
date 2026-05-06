---
layout: experiment
title: "Introducing Automated Change Log Tracking"
date: 2026-05-06
categories: ["AI & Machine Learning", "Developer Experience"]
excerpt: "We built an automated solution powered by AgentPanda that scans predefined vendor sources, summarises relevant updates and delivers concise change logs directly into Slack. A structured, low-effort way to stay informed about technology developments that matter to Bitpanda."
action: "Learn more"
---

# The Challenge

At Bitpanda, we work with a broad ecosystem of external vendors and open-source technologies. Each of these tools evolves continuously, releasing new features, security patches, compliance updates and performance improvements. While these updates are valuable, tracking them manually across multiple platforms is inefficient and difficult to scale. Important changes can easily be overlooked, and teams may miss opportunities to leverage new capabilities.

# Introducing Automated Change Log Tracking

To address this challenge, as a Labs project we introduced an automated solution powered by **AgentPanda** which is our internal AI-powered Slack bot built on top of the **OpenAI Agents SDK**. Instead of relying on manual monitoring, AgentPanda now automatically scans predefined vendor sources, summarises relevant updates and delivers concise change logs directly into Slack. The result is a structured and low-effort way to stay informed about technology developments that matter to Bitpanda.

# How It Works

Every Tuesday, AgentPanda reviews selected vendor sources and publishes summarised updates into domain-specific Slack channels. These channels are organised by functional area, allowing teams to subscribe only to the updates relevant to their work. This keeps information targeted and actionable rather than overwhelming.

Currently, updates are distributed across the following channels and more:

- `#vendor-ai`
- `#vendor-devops`
- `#vendor-engineering`
- and more…

# The Takeaway

This automation transforms vendor monitoring from a reactive, manual task into a systematic capability. Teams benefit from faster awareness of new features, improved visibility into security and compliance changes, and greater transparency across domains. Most importantly, it removes operational overhead while strengthening Bitpanda's ability to adapt quickly to external innovation.

# What's Next

With this initiative, Labs continues to focus on reducing friction through intelligent automation. By centralising vendor intelligence and embedding it directly into existing workflows, Bitpanda ensures that critical updates are visible, digestible and actionable - without adding new processes or complexity.