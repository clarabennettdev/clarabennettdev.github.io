---
layout: post
title: "Rendering Markdown in the Terminal with Glamour"
date: 2026-02-20
---

So I've been on a bit of a terminal UI kick lately, and one thing that kept bugging me was how ugly markdown looks when you just dump it raw into the terminal. You know the drill — you're building a CLI tool and want to show a README or changelog, but it comes out as a wall of `#` symbols and `**` asterisks. Not great.

Then I found [glamour](https://github.com/charmbracelet/glamour) from the Charmbracelet folks, and honestly it's one of those libraries that makes you wonder why you didn't look for it sooner.

Glamour takes markdown and renders it with actual colors, bold text, italics, code blocks with syntax highlighting — the whole deal. It's the same engine that powers `glow` (their standalone markdown viewer), but packaged as a library you can drop into your own Go programs.

Here's how simple it is to get started:

```go
package main

import (
	"fmt"
	"log"

	"github.com/charmbracelet/glamour"
)

func main() {
	md := `# Hello from the Terminal

This is **bold**, this is *italic*, and here's some code:

` + "```go\nfmt.Println(\"rendered with glamour!\")\n```" + `

Pretty neat, right?
`

	out, err := glamour.Render(md, "dark")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Print(out)
}
```

That's it. One function call. The second argument is a style — `"dark"`, `"light"`, `"notty"` (for piped output), or `"auto"` to detect the terminal background. You can also build custom styles if the defaults don't match your vibe.

Where this really shines is in CLI tools. I've been using it to render `--help` output from markdown templates, display changelogs on `update` commands, and even show formatted error guides. Instead of maintaining two copies of documentation — one for the web, one for the terminal — you write markdown once and glamour handles the rest.

A couple things worth noting: glamour wraps text to a default width (usually 80 columns), which you can customize with `glamour.RenderWithEnvironmentConfig()` or by creating a `TermRenderer` with options. It also handles tables, blockquotes, and links gracefully — links get rendered with the URL visible since, you know, you can't click things in a terminal.

If you're building anything in Go that outputs text to humans, give glamour a look. It's one of those small additions that makes your tool feel surprisingly polished.
