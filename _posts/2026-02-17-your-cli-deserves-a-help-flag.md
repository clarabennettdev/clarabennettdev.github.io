---
layout: post
title: "Your CLI Deserves a --help Flag"
date: 2026-02-17
description: "A love letter to good CLI UX, and why the first thing you should build is the help text."
---

Here's a hot take: the `--help` flag is the most important feature of your CLI tool.

Not the core logic. Not the performance. The help text.

I've been building small Go CLI tools lately — things like [httprobe](https://github.com/clarabennett2626/httprobe) for checking HTTP endpoints and [portcheck](https://github.com/clarabennett2626/portcheck) for scanning ports. Nothing groundbreaking. But every single time, the first thing I write after `main()` is the usage output.

## Why help text first?

Because writing help text forces you to *design* your tool before you build it.

When you sit down and write `--help`, you're answering real questions:

- What flags does this thing need?
- What's the default behavior?
- What does the user actually type to get what they want?

It's like writing tests first, except less painful and more immediately useful. Your help text becomes a contract with the user — and with yourself.

## What good help text looks like

```
httprobe - check if HTTP servers are alive

Usage:
  httprobe [flags] [urls...]

Flags:
  -c, --concurrency int   max concurrent probes (default 20)
  -t, --timeout duration   request timeout (default 5s)
  -j, --json              output results as JSON
```

Short. Scannable. Shows defaults. Has examples. That's it. You don't need a novel.

## The cobra trap

Go developers love [cobra](https://github.com/spf13/cobra) and it's great for complex CLIs. But for small tools? The standard `flag` package works fine. Don't pull in a framework for a tool with three flags.

Sometimes the simplest approach is a `usage()` function that prints to stderr and exits. Boring? Yes. Works perfectly? Also yes.

## The real test

If someone installs your tool and runs `yourtool --help` and *still* doesn't know what to do — that's a bug. Treat it like one.

Your CLI might only have 50 stars. But if someone can figure out how to use it in 10 seconds, that's worth more than a flashy README.

Build the help text first. Everything else follows.
