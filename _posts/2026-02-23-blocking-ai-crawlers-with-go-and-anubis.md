---
layout: post
title: "Blocking AI Crawlers with Go and Anubis"
date: 2026-02-23
---

I've been mass-blocking AI crawlers via `robots.txt` for months now and honestly, it feels like yelling into the void. GPTBot, CCBot, Google-Extended — they don't all respect `robots.txt`, and new ones show up constantly. So when I stumbled on [Anubis](https://github.com/TecharoHQ/anubis) by TecharoHQ, I immediately wanted to try it.

Anubis is a Go reverse proxy that sits in front of your web server and weighs the soul of incoming HTTP requests - yes, like the Egyptian god. It uses a proof-of-work challenge to decide whether a visitor is a real browser or just a scraper bot doing raw HTTP at scale. Normal browsers chew through a little JavaScript challenge without you noticing. The dumber crawlers that just fire requests and never execute JS get stopped at the door. And it's in Go, which I like because it usually means "here's a binary, go run it" instead of "install 14 things and pray."

Getting it running with Docker is dead simple:

```yaml
services:
  anubis:
    image: ghcr.io/techarohq/anubis:latest
    environment:
      - BIND=:8923
      - TARGET=http://your-app:3000
      - DIFFICULTY=5
    ports:
      - "8923:8923"
```

Point your reverse proxy (Caddy, nginx, whatever) at Anubis on port 8923 instead of directly at your app. Anubis forwards legit traffic to your actual backend. That's it.

The `DIFFICULTY` setting controls how hard the proof-of-work challenge is. Higher means more CPU cycles for the client to solve it, which real browsers barely notice but makes scraping way more expensive if you're trying to do it at scale with headless requests. Default felt fine to me, but if you're getting slammed you can turn the knob up and make it suck a lot more for the people doing the scraping.

What I like about this approach versus `robots.txt` is that it doesn't rely on bots being polite. It's not a suggestion, it's a gate. If you can't run JavaScript and solve the challenge, you don't get in. Period.

The project has blown up recently - thousands of stars on GitHub - and yeah, I get why. AI companies have been really aggressive about scraping, and the old bag of tricks (`robots.txt`, rate limiting, user-agent blocking) is basically speed bumps. Anubis actually forces the client to do work, and that changes the math.

If you're running anything in Go or Docker (so, basically everyone), there's no reason not to drop this in front of your site. It took me about ten minutes to set up and I haven't looked back.
