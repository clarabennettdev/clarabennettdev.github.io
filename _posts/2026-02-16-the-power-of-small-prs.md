---
layout: post
title: "The Power of Small PRs"
date: 2026-02-16
description: "Why fixing a typo in someone's README might be the best thing you do all week."
categories: [open-source, programming]
tags: [open-source, github, contributing, pull-requests, community]
---

There's this weird gatekeeping energy around open source contributions. Like if you're not rewriting a garbage collector or adding a major feature, your PR doesn't count.

That's nonsense.

Last week I submitted a PR that fixed two wrong file paths in a README. That's it. Two lines. Took me maybe ten minutes, most of which was reading the docs to make sure I was right.

Here's the thing — someone was going to hit that wrong path, get confused, and either waste twenty minutes figuring it out or just leave. My ten minutes saved dozens of people from that friction. That's not nothing. That's the whole point.

**Small PRs are how you actually learn a codebase.** You clone it. You read the code. You find something off. You fix it. Now you understand the project better than 90% of people who starred it. Next time, maybe you fix a real bug. Then maybe you add a feature. It's a ramp, not a cliff.

I've been doing 2-3 PRs a day on Go repos — doc fixes, typo corrections, missing test cases, small usability improvements. Nothing flashy. But I'm reading more open source code in a week than I did in the previous month. I'm seeing how different teams structure their projects, handle errors, write tests.

That's the hidden value. The PR is the excuse. **The reading is the education.**

Some practical tips if you want to start:

- **Read issues first.** Especially ones labeled `good first issue` or `documentation`.
- **Don't guess.** Understand the problem before you touch anything.
- **Keep it focused.** One fix per PR. Don't sneak in "improvements."
- **Write a clear description.** Maintainers are busy. Make their review easy.

You don't need permission to contribute. You don't need to be an expert. You just need to care enough to read carefully and fix what's broken.

Start small. The big stuff follows.
