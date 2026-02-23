---
layout: post
title: "Generic Methods Are Coming to Go"
date: 2026-02-23
---

So the generic methods proposal for Go just hit "likely accept" and I'm genuinely excited about this one. If you've been writing Go generics since 1.18, you've probably run into the same wall I have: you can't define methods with their own type parameters. That's about to change, and honestly it fixes a bunch of annoying API weirdness.

Right now, if you want a `Map` method on a custom collection type, you're kind of stuck. You can write a generic function that takes your collection and a transform func, sure. But you can't hang it off the type as a method. So you end up with these half-and-half APIs where some stuff reads nicely as methods and then you randomly have to drop out to a package-level function for the one operation that changes types. Not great.

Here's what we'll be able to do:

```go
type Stream[T any] struct {
    items []T
}

func (s Stream[T]) Map[U any](fn func(T) U) Stream[U] {
    out := make([]U, len(s.items))
    for i, v := range s.items {
        out[i] = fn(v)
    }
    return Stream[U]{items: out}
}

func (s Stream[T]) Filter(fn func(T) bool) Stream[T] {
    var out []T
    for _, v := range s.items {
        if fn(v) {
            out = append(out, v)
        }
    }
    return Stream[T]{items: out}
}
```

And then chain them naturally:

```go
names := NewStream(users).
    Filter(func(u User) bool { return u.Active }).
    Map(func(u User) string { return u.Name })
```

That fluent style has been basically impossible to do cleanly until now. The moment you needed a type-changing operation, you had to break the chain and call some `stream.Map()` helper or whatever you named it. This fixes that.

**What this unlocks:**

- **Better collection libraries.** Types like Stream, Result, Option can finally have ergonomic method-based APIs with full type transformations. No more "why is `Filter` a method but `Map` is a free function" nonsense.
- **Cleaner interfaces.** You'll be able to define interfaces with generic methods, which opens the door to proper functor/monad-style patterns without the current workarounds. If you've ever tried to model this stuff in Go today, you know how many little compromises it turns into.
- **Library design improvements.** If you maintain a library with generic types, start thinking about which package-level functions would be better as methods. A bunch of APIs are going to look kinda dated once this lands.

**How to prepare:**

If you're writing libraries with generic types today, I'd keep your package-level transform functions, but I'd design your types so adding methods later is non-breaking. Don't paint yourself into a corner with concrete return types when you can already tell "yeah, that's going to want a type parameter later."

I'd also hold off on any "generic collections framework" until this actually lands - the API surface is going to look completely different once generic methods are on the table.

This is the piece that was missing from Go generics. Can't wait to see it ship.
