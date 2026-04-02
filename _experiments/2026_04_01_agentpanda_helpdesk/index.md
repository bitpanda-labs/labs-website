---
layout: experiment
title: "AgentPanda Helpdesk: Turning Plain English into Service Requests"
date: 2026-04-01
categories: ["AI & Machine Learning", "Developer Experience"]
excerpt: "We built a Slack command that turns plain-language descriptions into fully categorised, pre-filled Jira Service Management tickets. No more hunting through service catalogs, just describe what you need and AI does the rest."
action: "Learn more"
---

AgentPanda is our internal AI-powered Slack bot built on top of the OpenAI Agents SDK. It acts as an assistant for Bitpanda employees, handling everything from vendor update lookups and Jira ticket management to scheduled reporting tasks. Each capability is exposed as a tool that the agent can invoke depending on what the user asks. The helpdesk command is one of its newest additions and the one that solved a friction point almost everyone at the company had experienced.

# AgentPanda Helpdesk: Turning Plain English into Service Requests

Nobody enjoys filing helpdesk tickets. You know what you need, but you are never sure which ticket you should fill out. Whether it's a new monitor, a permission change, a VPN reset, you first have to figure out which service desk handles it, pick the right request type from a list of dozens, and then fill in a detailed form. We wanted to skip all of that.

We built a Slack command that takes a plain-language description of what you need and turns it into a fully categorised, pre-filled service request. No more guessing which catalog to open or which form to fill out.

# The Problem with Service Catalogs

Jira Service Management is a powerful platform, but finding the right form is often a challenge. A typical enterprise setup has multiple service desks, each with its own set of request types, and each request type with its own unique form fields. For employees, this means navigating a maze of catalogs just to ask for a keyboard. People often end up messaging someone on Slack anyway, asking "where do I file this?". We decided to meet users where they already are. If a request starts as a Slack message, it should be able to end there too.

# How It Works

The flow is designed to feel like a conversation, not a form. A user types /open-helpdesk-ticket in Slack and gets a simple modal: "Describe your request." That is it.

![Helpdesk request modal in Slack](/experiments/2026_04_01_agentpanda_helpdesk/img/helpdesk_request.png)

Behind that simplicity is a multi-step AI pipeline. First, we pull the full catalog of available service desks and request types from the Jira Service Management API. Then we feed the user's description along with all available request types into an LLM using structured output. The model returns the best-matching request type and up to two alternative suggestions.

Once we have a match, we fetch the specific form fields for that request type and run a second LLM call. This time, the model extracts field values directly from the user's original description.

![Pre-filled helpdesk form](/experiments/2026_04_01_agentpanda_helpdesk/img/helpdesk_form.png)

The user sees a confirmation screen showing the matched request type, the pre-filled fields, and a dropdown to switch to an alternative if the AI got it wrong. One click later, they land on the Jira portal with everything already filled in. Review, submit, done!

![Open in Jira portal](/experiments/2026_04_01_agentpanda_helpdesk/img/open_in_portal.png)

# Keeping It Fast

Latency matters when you are staring at a loading screen inside a Slack modal. The Jira Service Management API can be slow, especially when fetching catalogs with dozens of request types across multiple desks. We added a ten-minute in-memory cache for the service desk catalog, so repeated requests do not wait for a full API round-trip. The LLM calls are the real bottleneck (two sequential inference calls per request!) but by keeping the prompts focused and using structured output with tight Pydantic schema, we keep the total flow under a few seconds.

# What We Learned

The biggest surprise was how well structured output handled the messiest part of the problem: field extraction. Users describe things in wildly inconsistent ways. "Next Monday," "03/15," "mid-March" all need to become a valid ISO date. Dropdowns need fuzzy matching because a user will write "Vienna" when the option is "Vienna, AT." By giving the LLM clear instructions and constraining its output format, we got reliable extraction without building a custom NLP pipeline.

The other takeaway is that confidence scoring changed how we thought about the UX. Instead of forcing a single match, we suggest alternatives when the model is uncertain. This keeps the user in control without making them do the categorisation work themselves.

# What's Next

We have launched the Helpdesk command as a pilot for the IT Service Desk. This initial phase allows us to gather authentic user feedback, identify edge cases, and fine-tune the matching before scaling further. Once we are confident in the results, we plan to roll this out across all service desks to streamline the user experience and significantly reduce catalog-browsing time.
