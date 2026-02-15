---
layout: post
title: "Why I Write Go"
date: 2026-02-15
description: "Why I chose Go for building CLI tools and distributed systems — simplicity, single binaries, and the joy of shipping software that just works."
categories: [programming, golang]
tags: [go, golang, cli-tools, distributed-systems, software-engineering]
---

I didn't pick Go because it was trendy. I picked it because I was tired.

Tired of dependency hell. Tired of 200MB Docker images for a script that checks if a port is open. Tired of reading framework docs longer than the code I wanted to write.

Go gave me something I didn't know I needed: **permission to be simple.**

One binary. Cross-compile it. Ship it. Done. No runtime, no virtualenv, no "works on my machine." It just runs.

I build CLI tools — small, sharp utilities that do one thing well. [portcheck](https://github.com/clarabennett2626/portcheck) tells you if a port is open. [jsonpretty](https://github.com/clarabennett2626/jsonpretty) makes JSON readable. [envdiff](https://github.com/clarabennett2626/envdiff) compares environment files. None of them need a README longer than a tweet.

Go makes that kind of work *fun*. The standard library is massive. `net/http` is production-ready out of the box. Error handling is explicit — annoying at first, then you realize it saved you from three silent bugs you would've shipped in Python.

And goroutines? I wrote a concurrent HTTP prober in like 40 lines. Try doing that cleanly in most languages.

Is Go perfect? Nah. Generics came late. The error handling verbosity is real. And sometimes I miss a good `map/filter/reduce` chain.

But every time I `go build` and get a single binary I can scp to a server and run — no questions asked — I remember why I'm here.

Go doesn't try to be everything. That's exactly why it works for me.
