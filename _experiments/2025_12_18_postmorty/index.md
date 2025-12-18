---
layout: experiment
title: "How AI is Revolutionizing Our Incident Response"
date: 2025-12-18
categories: ["AI & Machine Learning", "Developer Experience", ]
excerpt: "When something goes wrong with a service you rely on, you want it fixed fast. But what happens after the fix?"

action: "Learn more"
---

# Why Postmortems Matter

Postmortems are structured reviews after an incident (like a service outage or bug in production). The idea is simple: analyse what happened, understand why, and define clear steps to prevent it in the future. 
At Bitpanda these are mandatory to make sure we're always improving. They're not about pointing fingers, they’re about understanding what went wrong and how to prevent it from happening again.


# The Challenge

Traditionally, writing a postmortem can take **up to 60 minutes**. It involves gathering timelines, combing through Slack threads, pulling logs, calculating financial impacts, and coordinating across multiple teams. 
All of this data lives in different places and engages multiple developers and managers. With the increasing complexity of our systems and regulatory requirements, this manual approach was becoming a significant bottleneck.

# Introducing PostMorty

Our AI team developed PostMorty to solve this exact problem. It’s an AI-powered Slack bot that helps our teams **automatically generate** postmortems.

How does it work?

With just a simple Slack Command, PostMorty will start working on the task.

![Work in progress notification in slack](/experiments/2025_12_18_postmorty/img/01_in_progress.png)

Users also have the opportunity to add any other additional notes, logs, meeting notes, etc.

![Upload additional information screen](/experiments/2025_12_18_postmorty/img/02_upload_additional_info.png)

And just in a few seconds it’ll reply in the channel with a comprehensive Postmortem that follows our internal guidelines.

In summary, PostMorty does the following:
1. **Scrapes relevant context** from Slack and other platforms to capture what happened during an incident.
2. **Identifies patterns and contributing factors** that may be overlooked in manual reviews.
3. **Extracts root causes** and highlights key moments in the incident timeline.
4. **Generates structured postmortem drafts** — complete with recovery steps, lessons learned, and corrective actions.

This automation means our engineers can focus on what they do best: building and maintaining a great platform for you.

# The Numbers Speak for Themselves

The results have been incredible. What used to take our team members around **60 minutes** to complete manually, PostMorty can now do in a matter of **15 seconds**. That's an efficiency gain of more than **240x**!

This huge time saving allows our teams to:
- **Resolve issues faster:** By quickly understanding the root cause, we can implement corrective actions more efficiently.
- **Maintain higher quality:** With less manual work, our teams can focus on the most complex incidents, ensuring every detail is captured and addressed.
- **Focus on innovation:** Our engineers get to spend more time building the next generation of features for Bitpanda.

# What's Next?
PostMorty is just one example of how we are leveraging AI to make our internal systems more robust and efficient. From **real-time incident triaging** to smarter operations, our AI team is working hard to make sure our technology evolves as fast as the financial world does.

We’ll keep sharing more behind-the-scenes updates, because we believe building trust means showing how we build, too.