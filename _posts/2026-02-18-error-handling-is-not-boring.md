---
layout: post
title: "Error Handling Is Not Boring"
date: 2026-02-18
categories: [go, opinion]
---

I've seen the memes. "If err != nil" repeated fifty times. Go's error handling is "verbose." It's "tedious." It's "boring."

I used to agree. Then I spent a year debugging production services and changed my mind.

## The thing nobody talks about

Every `if err != nil` is a decision point. What do you do with the error? Wrap it? Log it? Return it? Retry? Each one forces you to *think* about failure at the exact moment it might happen.

Compare that to languages where you throw exceptions and hope someone catches them three stack frames up. Or worse — nobody catches them, and your service dies with a stack trace that tells you nothing useful.

```go
f, err := os.Open(path)
if err != nil {
    return fmt.Errorf("loading config from %s: %w", path, err)
}
```

That's not boilerplate. That's context. When this error surfaces in a log at 3am, you'll know *exactly* what was happening and why.

## Wrapping is an art

The real skill isn't handling errors — it's wrapping them well. A chain like:

```
starting server: loading config from /etc/app.yaml: open /etc/app.yaml: permission denied
```

...tells you the whole story. No stack trace needed. No debugger needed. You read it left to right and you *know*.

I started treating error messages like commit messages. Be specific. Add context. Future-you will be grateful.

## The honest take

Is it verbose? Yeah. Does it produce more lines of code? Absolutely. But those lines aren't wasted — they're insurance. Every wrapped error is a breadcrumb you're leaving for the person who has to debug this at 3am.

And that person might be you.

So next time you type `if err != nil`, don't sigh. You're doing the work that matters.
