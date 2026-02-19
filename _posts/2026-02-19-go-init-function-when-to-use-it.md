---
layout: post
title: "Go's init() Function: When to Use It (and When Not To)"
date: 2026-02-19
categories: [go, programming]
---

Let's discuss Go's `init()` function—it's convenient until it causes issues.

For those unfamiliar, `init()` is a special function that runs automatically when a package is loaded, before `main()` executes. You don't call it; it just happens. You can have multiple `init()` functions in the same package, and they will all run.

There are valid uses for it, such as database driver registration:

```go
// This is the standard pattern — and it's fine
import _ "github.com/lib/pq"
```

This blank import triggers `pq`'s `init()`, which calls `sql.Register()` to make the Postgres driver available. It's a well-known convention. Setting package-level defaults that are constant and side-effect-free is another acceptable scenario.

However, I've had my share of issues with `init()`. Here's why:

**Testing becomes difficult.** You can't skip `init()`. When you import a package, its `init()` runs, even in tests. If that function connects to a database or reads a config file, your unit tests will have external dependencies.

**Hidden side effects.** When someone looks at your `main.go` and sees a clean `main()` function, they might not realize that deeper in the call stack, an `init()` is launching goroutines and opening network connections.

**Ordering surprises.** If you have `init()` functions across multiple files and packages, the execution order depends on import order and filename sorting. It's deterministic, but it can be tricky to understand in a large codebase.

I prefer a more straightforward approach:

```go
// Instead of this:
func init() {
    cache = connect(os.Getenv("REDIS_URL"))
}

// I'd rather do this:
func NewCache(url string) (*Cache, error) {
    return connect(url)
}

// Called explicitly in main:
func main() {
    cache, err := NewCache(os.Getenv("REDIS_URL"))
    if err != nil {
        log.Fatal(err)
    }
    // ...
}
```

Yes, it requires more code. But now I can test `NewCache` with a mock URL, I can clearly see when initialization occurs, and I can handle errors appropriately. Unlike `init()`, which can't return an error, forcing people to call `log.Fatal()` or `panic()` within it, leading to chaos.

My advice: if you're writing a driver that follows a common pattern, `init()` is acceptable. For nearly everything else, make it explicit. You'll thank yourself later.
