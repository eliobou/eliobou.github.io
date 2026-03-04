---
title: "How I Manage My Todolist with AI"
date: 2025-11-03
draft: false
tags: ["productivity", "ai", "automation", "todoist", "chatgpt", "workflow"]
categories: ["productivity", "development", "script"]
---

I've always had a complicated relationship with todo lists. You know the drill: you start with good intentions, add tasks as they come up, and then... nothing. Tasks pile up, priorities blur, and before you know it, you're looking at a 50-item list where everything feels equally urgent and equally impossible. Eventually, you just stop looking at it altogether.

For years, I cycled through this pattern. I'd try a new system, feel productive for a week or two, then watch it slowly decay into chaos. The problem wasn't the tools or my motivation. It was that I had no real method to keep things under control and no way to see what I was actually accomplishing.

<center><img src="todoist_projects.png" alt="Projects" style="width: 50%;"></center>

## The Daily Discipline: 5-7 Tasks

These days, I use Todoist for everything: work, personal stuff, and side projects. But here's the thing: I don't obsess over due dates when I capture tasks. When something comes up, I throw it into the appropriate project and move on. No stress, no overthinking.

The real work happens each morning. I go through my projects and select 5 to 7 tasks for the day. That's it. Not 3, not 15. This range is the sweet spot I've found where I stay productive without setting myself up for failure.

Why this number? It's enough to feel like I'm making real progress, but realistic enough that I can actually get it all done. I'm not just blindly stacking tasks anymore. Each selection is deliberate. It forces me to think about what truly matters today.

## The Two-Delay Rule

Of course, life happens. Sometimes a task doesn't get done. When that happens, I push it to the next day and start with it first thing in the morning. No big deal.

But if I push the same task twice? That's a red flag.

When a task gets delayed twice, I stop and ask myself some hard questions:
- Is this actually a priority, or am I just keeping it around out of habit?
- Is it realistically achievable, or do I need to break it down?
- Is there something blocking me that I haven't addressed?
- Why am I avoiding this?

Sometimes the answer is that the task isn't important anymore. Sometimes I realize I need more information or help from someone else. Other times, I'm just procrastinating on something difficult, and I need to face that head-on.

This simple rule has saved me from the endless cycle of carrying dead weight in my todo list. It's like a built-in bullshit detector for my own planning.

## The Weekly AI Summary

Here's where things get interesting. Every Sunday at 9 PM, [a script I wrote](https://github.com/eliobou/todoist-ai-summary) runs automatically. It pulls all the tasks I completed during the week via the Todoist API, sends them to ChatGPT, and asks it to generate a summary. The result lands in my inbox: a clean, organized overview of my entire week.

This might sound like a small thing, but it's become one of my favorite productivity tools.

First, there's the sense of accomplishment. When you're in the middle of a busy week, it's easy to feel like you're not getting anywhere. But seeing everything laid out in an email, categorized and summarized, gives you a completely different perspective. You realize you actually did a lot. That feeling matters.

Second, it gives me visibility into where my time actually goes. I can see patterns: maybe I'm spending too much time on administrative stuff and not enough on the technical work I enjoy. Or maybe those side projects are eating up more hours than I thought. Having this data helps me make better decisions about how to allocate my time going forward.

And the cost? Negligible. We're talking cents per year to run this through ChatGPT's API. It's probably the best ROI of any tool in my productivity stack.

<center><img src="github.png" alt="Github" style="width: 100%;"></center>

## Beyond Weekly Reflection

The benefits extend beyond just feeling good about your week. These summaries become an archive of everything you've done. When it's time for a weekly meeting at work, I don't have to scramble to remember what I accomplished. I just open last week's email.

During annual reviews? Same thing. I have a searchable record of an entire year's worth of work, organized and summarized. It's made those conversations so much easier. Instead of vaguely gesturing at "being productive," I can point to specific achievements and patterns of work.

For side projects, it's even more valuable. When you're working on something in your spare time, weeks can blur together. Did I actually make progress on that app, or did it just feel like it? The summaries don't lie. They show you exactly what moved forward and what stalled.

## What This System Actually Does

Let me be clear: this isn't about being some hyper-optimized productivity machine. I'm not tracking every minute or gamifying my task completion. I still have lazy days. I still procrastinate sometimes.

But this system gives me two things I never had before: clarity and confidence.

Clarity because I know what I'm doing today and why it matters. I'm not drowning in an endless list. I'm making conscious choices about my priorities.

Confidence because I can see my progress. Not in some abstract way, but in concrete terms. When imposter syndrome kicks in, I can look at those weekly summaries and remind myself that I'm actually getting shit done.

The AI summary isn't doing my work for me. It's just holding up a mirror so I can see what I've already accomplished. Sometimes that's exactly what you need.

## Getting Started

If this sounds interesting, you don't need to replicate my exact setup. The core principles work with any todo app:
- Limit your daily tasks to a reasonable number
- Have a rule for tasks that keep getting delayed
- Find a way to review your progress regularly

The AI part is optional, but I'd encourage you to try it. The [script is on GitHub](https://github.com/eliobou/todoist-ai-summary), and it's straightforward to adapt to your needs. Even if you just manually review your completed tasks each week, you'll be surprised at how much it changes your perspective.

The goal isn't perfection. It's sustainable progress. And I feel like I've found a system that delivers on that.