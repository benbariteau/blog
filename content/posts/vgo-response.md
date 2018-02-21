---
title: "vgo: at least they're doing something"
date: 2018-02-20T22:01:00-08:00
draft: false
tags:
    - golang
    - programming
    - vgo
---

Today Russ Cox, posted [this blog post](https://research.swtch.com/vgo-intro). When I first saw the title and who it was from, I was pretty excited! One of the biggest issues with go since the beginning has been the lack of support for any meaningful kind of versioning. The most thought given to it was the go1 compatibility rule: which is to say, never break your API. Strangely enough this didn't work out.

So here we are, nearly 6 years after go hit 1.0, we're finally getting an actual proposal from one of the big go people about versioning. And if you have an experience with the go designs, you won't be surprised to hear it's a fucking weird one.

# The Good
First and foremost, something is actually being done! Since the beginning, the go team claims that they weren't sure how to approach versioning. This is despite the fact that there are copious examples of versioning to draw examples from. (Perhaps they weren't sure how to be arbitrarily contrarian about it!) Regardless, this is a milestone in the community. A lot of time was spent saying "don't break your APIs" or "use gopkg.in" or "just check in your deps", so this is like a breath of fresh air.

The fact that semver is being used - and being treated largely as a "duh" kind of thing - is excellent. Also, the fact that tags are considered the primary way of thinking about versions is a great initial design decision. And even though SHAs are supported, they are set up to require a datetime for ordering and are considered the lowest possible versions. There are some caveats to this, but I'll get to them below.

The module manifest format is pretty good! No version which can easily confuse build systems. I wish it had been an actual config format rather than a weird code-like thing. I also feel that the lack of anything but >= as a version specifier is going to bite someone in the ass someday.

The zip distribution format makes a lot of sense! I'm glad it's so simple and also has legitimate uses. I hope that it doesn't become a dumping ground for generated code, though.

Strangely buried - and seemingly tangential - is removal of the GOPATH requirement. This has long been a stumbling block for developers (newcomers especially) because of how different it is from all other language's paradigms. We first saw inklings of this possibly with the [vendor changes](https://blog.golang.org/go1.5) in go 1.5. The details on this are sparse, but it goes a long way for go to be not weirdly different from other languages.

# The Bad
Remember when I mentioned the go design being arbitrarily contrarian? (generics, anyone?) Well, "minimal version selection" (confusing name? check. sounds official even though it's made up? double check.) is that design. In short, instead of picking the newest version that fits the versioning constraints, vgo will pick the _oldest_ one. The claimed reasons for this is that it guarantees reproducible builds without requiring a lockfile, and it also makes sure that your builds never break because a new version comes out. Frankly, the zeroconf zealotry here is exactly what led us to `go get` in the first place, so I'm not sure why the go team continues to think that's a good goal. Luckily, this is a design decision that can be changed in the future, so hopefully it will once people realize how useless it is.

However, the dumb doesn't end there. The "import compatibility rule" is one of the "new" conventions that is supposed to be used in this system: if when doing a major release (backwards-incompatible), then you must create a new import path (e.g. "foo.com/bar" would become "foo.com/bar/v2"). I can't even fathom who thought this made sense. The whole point of having versioning is so that don't have to version elsewhere. This gopkg.in all over again but somehow even less maintainable.

# The Weird
The fact that a second command, `vgo` is being added to the toolchain is really weird to me. I guess it makes sense to roll this out, but why not just do it like the vendor dir rollout, with a env variable? It'll probably be moot in a year anyway, so this isn't a big deal.

The fact that `dep` is being deprecated (lol) is very strange to me as well. I sort of get the reasoning of just having the essential features, but it seems like such a weird thing to do with a tool with so much community input and investment.

# TL;DR
Versions good, golang weird.
