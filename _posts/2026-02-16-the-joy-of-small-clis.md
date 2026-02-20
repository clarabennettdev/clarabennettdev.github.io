---
layout: post
title: "The Joy of Small CLIs"
date: 2026-02-16
---

There's something deeply satisfying about writing a CLI tool that does exactly one thing.

Not a framework. Not a platform. Just a little program that takes input, does a job, and gets out of the way. I've been building a few of these in Go lately — [httprobe](https://github.com/clarabennett2626/httprobe), [portcheck](https://github.com/clarabennett2626/portcheck), [jsonpretty](https://github.com/clarabennett2626/jsonpretty) — and every time, the process reminds me why I fell in love with programming in the first place.

## Why small?

Big projects are important. But small tools teach you things that big projects hide behind abstractions. When your entire program is 200 lines, every decision matters. How do you handle stdin vs file arguments? What's the default behavior when no flags are passed? Does it play nice with pipes?

These aren't glamorous questions, but they're the difference between a tool people actually use and one that collects dust.

## Go makes it easy (and fun)

Go is almost unfairly good at this. Cross-compile to any platform with `GOOS` and `GOARCH`. Single binary, no runtime dependencies. The `flag` package is minimal but honest. And if you need something fancier, `cobra` is right there.

But honestly? For small CLIs, I usually stick with the standard library. There's a certain discipline in not reaching for dependencies when you don't need them.

## The Unix philosophy isn't dead

Every time I pipe the output of one small tool into another, I feel like I'm participating in a tradition. `cat file.txt | httprobe | grep 443` — that chain works because each tool minds its own business.

We don't need more monoliths. We need more tools that do one thing well, print their output cleanly, and exit with the right status code.

If you haven't built a small CLI lately, I'd recommend it. Pick a problem that annoys you, solve it in under 300 lines, and ship it. You might surprise yourself with how good it feels.
