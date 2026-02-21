---
layout: post
title: "Modernize Your Go Codebase in One Command with go fix"
date: 2026-02-21
---

So I've been mass-upgrading a handful of Go services at work this week, and I stumbled onto something that made me mass-upgrade *all* of them: the `go fix` tool in Go 1.26 now ships with 24 built-in modernizers, and honestly it's kind of magical.

If you haven't seen it yet, running it is dead simple:

```bash
$ go fix -fix modernize ./...
```

That's it. One command, and it sweeps through your entire module looking for outdated patterns it can safely replace with modern equivalents. No config files, no third-party linters — it's right there in the toolchain.

Here's the kind of stuff it catches. Say you've got code like this scattered around:

```go
func process(data map[string]interface{}) {
    var result []string
    for i := 0; i < len(data); i++ {
        if strings.Index(name, "@") != -1 {
            // ...
        }
    }
}
```

After `go fix`, you get:

```go
func process(data map[string]any) {
    var result []string
    for i := range len(data) {
        if strings.Contains(name, "@") {
            // ...
        }
    }
}
```

`interface{}` → `any`. Integer range loops using `range len(...)` (Go 1.22+). `strings.Index != -1` replaced with `strings.Contains`. All automatic, all correct.

A few of my favorite fixers from the 24 built-in ones:

- **`efaceany`** — Replaces `interface{}` with `any` everywhere. This one alone touched 200+ files in our biggest service.
- **`stringscutprefix`** — Spots the classic "check HasPrefix then TrimPrefix" pattern and collapses it into `strings.CutPrefix`.
- **`sortslice`** — Swaps `sort.Slice` calls for `slices.SortFunc` from the newer `slices` package.
- **`mapsloop`** — Replaces manual map-to-slice loops with `maps.Keys` or `maps.Values`.
- **`rangeint`** — Converts `for i := 0; i < n; i++` to `for i := range n`.

What I really appreciate is that each fixer targets a specific Go version, so it only applies changes your `go.mod`'s `go` directive supports. Bumping from `go 1.21` to `go 1.26` in `go.mod` and then running `go fix` is a surprisingly satisfying workflow.

I ran it across six services this week. Zero test failures. The diffs were huge but entirely mechanical — great PRs to review because there's nothing subtle going on. If you've got any Go code that predates 1.22, give it a shot. You'll be surprised how much cleaner things look on the other side.
