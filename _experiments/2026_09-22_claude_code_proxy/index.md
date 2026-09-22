---
layout: experiment
title: "How We Brought Cost Control to Claude Code"
date: 2026-09-22
categories: ["AI & Machine Learning", "Developer Experience"]
excerpt: "When we rolled out Claude Code to our engineering team via AWS Bedrock, we quickly ran into a gap: unlike using Claude through Anthropic’s enterprise offering, Bedrock didn’t give us out-of-the-box visibility into per-engineer spend or a way to enforce individual usage limits. To solve this, we built a FastAPI proxy that sits between Claude Code and AWS Bedrock, handling identity, budget enforcement, and audit logging without engineers noticing any difference."
action: "Learn more"
---

When we rolled out Claude Code to our engineers via AWS Bedrock, we quickly ran into a challenge that had little to do with the tool itself: cost visibility and control. Bedrock gave us one central bill, but unlike Anthropic’s enterprise offering, it didn’t provide out-of-the-box visibility into per-engineer spend or a way to enforce individual usage limits before costs accumulated.
For a small group of users, that might be manageable. At scale, it becomes a bigger problem. One engineer might use Claude Code occasionally, while another could run much larger workflows and generate significantly more spend.
We needed three things: visibility into who was using what, control over how much could be spent, and a frictionless experience for engineers. Most importantly, we wanted to introduce those guardrails without making Claude Code harder to use.

# What we built

We introduced a lightweight control layer between Claude Code and AWS Bedrock. From an engineer's perspective, almost nothing changed. They continue to use Claude Code in exactly the same way. Behind the scenes, our service identifies who is making each request, checks whether they still have a budget available, tracks what the request costs, and records the usage. That meant we could answer a few simple but important questions: who is using Claude Code, how much are they spending, and are they still within the limits we've set? Instead of only understanding our AI spend when the monthly bill arrived, we could now manage it as it happened.

# Identity without extra credentials

One of our biggest priorities was avoiding additional friction for engineers. We could have created a separate API key for every user, but that would have meant another credential to distribute, manage, rotate, and revoke. Instead, we use the AWS identity engineers already have. When someone uses Claude Code, our service can securely determine who they are and associate their usage with them automatically. There is no additional login, no new API key, and no manual onboarding process. For engineers, the experience remains simple: open Claude Code and start working. For us, every request now has an accountable owner.

# Setting sensible budgets

Once we could identify individual users, we could start managing spend at the right level. We didn't want to create a rigid system where everyone received exactly the same allowance. Different people and teams have different needs, especially around launches, projects, or periods of heavier development. So we introduced several layers of control. Every engineer receives a default monthly allowance. People who need more can be given a higher limit, and we also maintain an overall company-wide ceiling as a final safety net. If someone reaches their limit, the request is stopped before additional spend occurs.
Just as importantly, those limits can be adjusted immediately. If an engineer needs more capacity for an important project, the responsible team can increase their budget without waiting for a deployment or asking an engineering team to make a code change.

# Managing costs before they happen

One challenge with generative AI is that you don't always know exactly how much a request will cost until it has finished. But checking the budget only after the request has completed would be too late. So we use a model similar to the pre-authorisation process used in payments. 
Before a request starts, we reserve an estimated amount from the user's available budget. Once the request finishes, we replace that estimate with the actual cost. This allows us to make a decision before spending happens while still keeping the final accounting accurate. 
From the engineer's perspective, none of this is visible. Claude Code continues working normally in the background.

# What we learned

The key was making cost control flexible without adding friction. By keeping the guardrails in the background and making limits easy to adjust, we created a setup that could scale with adoption and changing business needs. 