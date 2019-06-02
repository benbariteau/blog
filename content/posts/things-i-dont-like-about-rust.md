---
title: "things I don't like about rust"
date: 2018-02-22T18:33:00-08:00
draft: false
tags:
    - rust
    - programming
---
So I want to preface this with the fact that I love Rust. It's currently my favorite language by not a small margin. If nothing else, it's the most interesting language in years given the fact that it's the first one to actually have compile-time memory safety. If you haven't checked it out, I highly suggest it.

# Errors

This is the biggest pain point that doesn't really have an upside. Errors in rust are just plain annoying. The biggest reason is that converting between error types is a pain. I usually end up using the [error-chain crate](https://crates.io/crates/error-chain) in my own code, but it ends up adding a bunch of boilerplate in order to even do errors in the sorta-wrong-but-easy way. Rust being an immature language means that patterns and libraries will probably be developed over time, but for now it's pretty annoying.

# Strings

I get it, systems programmers. _Technically_ it's more efficient to not copy strings. But you know what? Every other language does it and it works just fine. I would love it if we could just have a pragma that makes me not have to care when strings get copied or whatever.

The two types thing is bullshit too. Why can't `&str` just be `&String`? Every time a write a function or create a struct type that needs a string of some kind, it's so hard to decide what it should be. I guess I could use AsRef, but that's just needlessly confusing.

# Method culture

Functions are great. They're even better in Rust. But all of the functionality in Rust seems to be in methods. (At least you can't mutate internal state without a `mut` binding though!) The biggest reason I don't like this is that it encourages polymorphic shit even when it's not appropriate. Remember error-chain? It generates a Trait called ResultExt, which adds methods to the Result type, things which could easily be functions (especially since it's generated).

# Too many Traits

This is related to the method culture issues. [diesel](http://diesel.rs) is one of my favorite rust libraries. It's a really good ORM. However, when you try reading the type signature of its stuff, it gets extremely confusing, because it's a mess of Traits, instead of concrete types.

# Module structure

Once you get bigger than one file, getting symbols in the right place is super annoying. The fact that there's no good way to split a file in two without having to do a bunch of re-imports makes it something you avoid doing, which is the last thing you should be doing with your code.

# Import syntax

`extern crate`, `use`, and `mod` all being different (but for some reason necessary) keywords is confusing to say the least. I know that a planned feature is removing the `extern crate` keyword and just having it be `use`. But for now it's a pain, and `mod` is here to stay afaict.

`#[macro_use]` is a really annoying syntax, because it makes it really difficult to group your imports in a reasonable way. Not to mention the fact that it's an _everything_ import by default. We should definitely not be using glob imports anywhere, least of all for macros, which are hard to understand as is. Also, since macros don't have transient imports (if foo calls bar, then you have to import foo and bar), it encourages this bad behavior even more.

# That all being said...

I think a lot of these will be fixed by Rust becoming a more mature language (at least I hope so). Until then, I'll try my best to not contribute to these problems where possible.
