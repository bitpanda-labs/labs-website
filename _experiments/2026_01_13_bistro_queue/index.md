---
layout: experiment
title: "Bistro Queue: Solving Lunch Line Frustration with AI"
date: 2026-01-13
categories: ["AI & Machine Learning", "Company Culture"]
excerpt: "With 500+ employees sharing one bistro, lunch queues were getting chaotic. So we built an AI-powered queue monitor in a single evening."

action: "Learn more"
---

# The Daily Queue Dilemma

Picture this: it's 12:30 PM at Bitpanda's Vienna HQ. You're hungry, ready for lunch, and you walk toward the bistro. But as you approach, you see it: a line of colleagues stretching down the hallway. How long will you wait? 5 minutes? 15? 30?

With over 500 employees at our main office, plus colleagues visiting from other hubs, our bistro can get seriously crowded during peak coffee and lunch hours. The uncertainty led people to either gamble on the queue length or give up entirely, resulting in an uneven distribution throughout the day.

What if you could check the queue status in real-time, just like checking traffic before leaving home?

# A Hackathon Experiment

One evening, after work, two of us asked a simple question: could we build something useful to solve this in a single evening, rather than weeks or months?

Armed with Claude Code and a regular webcam, we set out to build **Bistro Queue**, a real-time queue monitoring system. The goal was simple: get a working proof of concept up and running before the end of the day.

![Hackathon experiment](/experiments/2026_01_13_bistro_queue/img/01_hackathon.png)

# How It Works

At its core, Bistro Queue uses computer vision to count people waiting in the bistro queue.

The Bistro Queue website displays:
- **Current queue length** (number of people waiting)
- **Expected waiting time** (estimated based on historical flow)
- **Queue statistics**: Average length and peak times for the last 15 minutes and last hour

This data helps employees decide the best time to grab their coffee or lunch without having to guess.

![Bistro Queue website](/experiments/2026_01_13_bistro_queue/img/02_bistro_site.png)

**1. Real-Time Person Detection**<br>
We integrated [YOLO](https://github.com/ultralytics/ultralytics) (You Only Look Once) models to detect and count people in the camera's view. The model processes the webcam feed continuously, identifying individuals in the frame with impressive accuracy.

**2. Defining the Queue Area**<br>
We quickly ran into a practical challenge: not everyone in the camera view is actually in the queue. Some people are leaving, some are sitting at tables, and others are just passing by.

We built a configuration panel that lets you draw a specific area on the camera feed. Only people within that zone count toward the queue length. This ensures we're only tracking the actual line, not the entire bistro.

![Bistro Queue configuration panel](/experiments/2026_01_13_bistro_queue/img/03_bistro_configuration.png)

# The Claude Code Magic

Here's what made this possible so quickly: **everything** was built with Claude Code.

When we say everything, we mean:
- Backend in Python with [YOLO](https://github.com/ultralytics/ultralytics) integration
- The vanilla HTML/CSS/JS frontend
- The server configuration (nginx setup)
- SSL certificates for secure access
- Deployment scripts

And when we say "built with Claude Code", we mean zero human code modifications. Every line of the implementation was written by Claude. No manual tweaks, no debugging sessions, no "let me just fix this one thing". From the first line of Python to the final nginx configuration, it was all AI-generated.

Claude Code didn't just help us write code; it set up the entire infrastructure. What would typically take a small team days or weeks of configuration and debugging was done in a single evening. This is the power of AI-assisted development: reducing the time from idea to working prototype dramatically.

# Privacy First

Before going live, we had an important question to answer: what about privacy?

The good news: Bistro Queue doesn't store any images, video recordings, or personally identifiable information. It only counts bodies in a defined area with no facial recognition, no tracking, and no individual data retention. Just a simple count that resets continuously.

# Real-World Impact

Today, Bistro Queue is **live and actively used** by our Vienna team. Several employees check it before heading to the bistro, and it's become a natural part of their daily routine.

The anecdotal feedback has been great: people appreciate knowing what to expect before committing to the walk, naturally distributing traffic throughout the day.

But beyond solving the immediate problem, Bistro Queue represents something bigger: **the speed at which we can now move from idea to production**. An evening hackathon resulted in a tool that's used by several employees daily. That's the kind of velocity that modern AI development tools unlock.

# What This Means for Innovation

Bistro Queue is a small example of a much larger shift. With tools like Claude Code, the barrier between "we should build this" and "this is live in production" has collapsed.

Gone are the days of needing weeks of planning, multiple developer sprints, and extensive infrastructure setup for simple internal tools. Now, a good idea and a couple of hours can result in something genuinely useful.

This is what Bitpanda Labs is all about: spotting opportunities, prototyping fast, and learning what works. Not every experiment will become a permanent feature, but every experiment teaches us something about what's possible.

This is the new reality of rapid prototyping and development.

And sometimes, the best solutions are the ones you build on a Tuesday evening.

# Full Circle: This Article Too

By the way, to complete the full cycle, **this article was also auto-generated by Claude**. Given the codebase, the initial requirements, and some additional context, Claude wrote this entire blog post. From idea to working system to published article, all in one seamless, AI-assisted workflow.
