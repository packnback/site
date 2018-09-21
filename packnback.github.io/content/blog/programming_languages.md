---
title: "Programming languages"
date: 2018-09-20T11:26:59+12:00
type: "blog_post"
draft: false
---

Choosing a programming language for a project is a compromise over what you
what you need, what you have, what you know and what you like. This post
is just my thought process when selecting the implementation language for packnback.

# What we need

- High performance, we will hopefully be dealing with large volumes of data.
- High security, the whole purpose is to protect data from attackers.
- Stability, software needs to be usable well into the future.
- Simplicity, the less complicated something is, the less that can go wrong.
- Popularity, this is mainly to help with marketing, libraries and community support.
- High bus factor, will sudden unexpected events destroy the language prospects.
- Fun, something we enjoy using or evaluating.

# What we have

The following ratings are just my unscientific and loose opinions, let me know if you disagree and why.
I skipped many choices because there isn't enough time to consider everything.
I mainly put languages I have some experience using or reading about.

### C

- performance 5 / 5
- security 0 / 5
- stability 5 / 5
- simplicity 5 / 5
- popularity 4 / 5
- bus-factor 5 / 5
- fun 2 / 5

total 26 / 35

In general, C would not be a bad choice if it werent for the track record
of security problems. Even if we could avoid them in our code, the reputation
damage is actually significant, 'Why is this in C?' questions mean it
gets a point off popularity.

C gurus can make amazing things happen with few lines of code, (partially because the
operating system is the garbage collector and abort() is the error handling) and in practice
many issues can be mitigated with compiler hardening and tool augmentation like valgrind.

edit:

It has been pointed out to me that C has lots of subtle complexity, and perhaps 5/5 for simplicity is not correct. I won't change
my initial numbers (to preserve a record) even if my mind might be changing on that point.

### C++

- performance 5 / 5
- security 2 / 5
- stability 5 / 5
- simplicity 1 / 5
- popularity 4 / 5
- bus-factor 5 / 5
- fun 0 / 5

total 24 / 35

I personally think C++ has gone off the complexity rails when it comes to
features. It works and is performant, and does better than C with security... but a
big problem is that I really do not enjoy writing C++ code anymore.

The levels of OO programming that is idiomatic C++ also bothers me at times. 
Sometimes a float is a float and doesn't need 100 lines of class boilerplate to wrap it.

### Java

- performance 4 / 5
- security 5 / 5
- stability 5 / 5
- simplicity 3 / 5
- popularity 5 / 5
- bus-factor 5 / 5
- fun 0 / 5

total 27 / 35

Java is pretty damn solid, I have also burned myself out on it.

Someone once described Java classes and code layout like shrink wrapping individual
grains of rice (but at least there is probably a 'Factory' or IDE to do it for you).

An honorable mention here is languages clojure that runs along side java as a 
library, which may be one of the most productive and highest level languages I know (more than python!)...
however it does have greatly reduced performance and sits at a far lower bus factor.

### [Python](https://www.python.org/)/[Ruby](https://www.ruby-lang.org/)

- performance 0 / 5
- security 4 / 5
- stability 5 / 5
- simplicity 3 / 5
- popularity 5 / 5
- bus-factor 5 / 5
- fun 5 / 5

total 27 / 35

These languages are slow, and distributing code written in them is painful and
complicated at best. They are fun and productive to use, though a bit error prone when
your lines of code get above around 10k and you start forgetting what exceptions things
can throw or don't have total test coverage.

There are probably more sucessful python or ruby projects than there are failed projects
in many of the other languages combined.

### [Go](https://golang.org/)

- performance 3.5 / 5
- security 5 / 5
- stability 4.5 / 5
- simplicity 3 / 5
- popularity 3.5 / 5
- bus-factor 4 / 5
- fun 4 / 5

total 27.5 / 35

Go is pretty rock solid, it works nearly everywhere, has quite a large
number of projects invested into it already,
has been stable for years and probably isn't going anywhere. Typing 'if err != nil' has
been giving me RSI recently and the bloated binary size and opaque runtime/memory 
management gives me some pause.

One huge pet peeve of mine is a language with concurrency but no notion of immutability. I have
been bitten by accidentally sharing a backing byte slice across function boundaries before and it
left me a bit bitter and paranoid. 

### [Nim](https://nim-lang.org/)

- performance 4.5 / 5
- security 4 / 5
- stability 3 / 5
- simplicity 3 / 5
- popularity 1 / 5
- bus-factor 1 / 5
- fun 5 / 5

total 21.5 / 35

Nim is a fun language, it performs extremely well from my 
tests, I think it is a shame that till now it was not more adopted.

One problem is that if the creator lost interest the project would grind to a total halt,
which may be a reason for the lack of adoption.

The macro system seems amazing, I immediately used it to eliminate boilerplate making safe bindings
to libsodium as a test and definitely enjoyed the process a lot.

Reading github issues and the forum, I stumbled across many plans from 2013-2016 that show
the project has some great ideas, but not enough man power. Reading the forum shows questions I had, asked by others, half answered and still undocumented.

Nim really does feel like the speed of C with the ease+fun of python. A positive sign
is the commit frequency is increasing over time, and they got sponsorship recently,
but the project is far from mainstream.

### [Rust](https://www.rust-lang.org)

- performance 5 / 5
- security 5 / 5
- stability 4 / 5
- simplicity 2 / 5
- popularity 3 / 5
- bus-factor 4 / 5
- fun 4 / 5

total 27 / 35

By all the metrics rust scores pretty well, my biggest concern is it will (or has) turn into
C++ 2.0 in a bad way, a complicated behemoth.

I don't find the code::that::appealing to look at including some minor things that
make the code hard to scan through by eye. Though I need to be sure it is not just my
own biases and non familiarity clouding my judgement. 

Fearless concurrency is definitely something that sells the language well to me, I feel very uneasy
looking at concurrent code other people wrote where I can't infer which threads have what invariants easily.
My understanding (which may be wrong) is with rust it doesn't really matter so much as the borrow checker rules also eliminate data races.

### [Zig](https://ziglang.org/)

- performance 5 / 5
- security 3.5 / 5
- stability 2 / 5
- simplicity 3 / 5
- popularity 2 / 5
- bus-factor 1 / 5
- fun 4 / 5

total 20.5 / 35

A great language focusing on robust software. The bus factor 
is an issue.

I would have loved if zig could just compile to more or less idiomatic C
and give projects an escape hatch if things aren't going well for zig development after all.

Zig is the only language I know to have a totally seamless C interop where you can include
C headers directly in zig code, other languages could learn from this.

From a security standpoint it has some of the C pitfalls
but can use bounds checking and things like arena allocators to mitigate manual memory management.

### [Myrddin](https://myrlang.org/)

- performance 3.5 / 5
- security 3.5 / 5
- stability 2 / 5
- simplicity 4 / 5
- popularity 1 / 5
- bus-factor 1 / 5
- fun 5 / 5

total 20 / 35

Much of what applies to zig applies to Myrddin, a solid and fun language.
It is however less popular right now and doesn't have windows support.

Myrddin does not use llvm and suffers in performance, but gains simplicity because of that.
It is a joy to build the myrddin compiler from source in a few seconds compared to 20+ minutes
that other langauges require.

Myrddin's type inference in a C like context really feels quite magical to use. My favourite
example was 'malloc()' knowing and returning the right pointer type and size which it inferred automatically from
another distant part of the code.

### [Haskell](https://www.haskell.org/)

- performance ? / 5
- security 5 / 5
- stability 4 / 5
- simplicity 3 / 5
- popularity 3 / 5
- bus-factor ? / 5
- fun ? / 5

total ? / 35

This is a bit of a wild card, I am not experienced at all with haskell, but my impression
is it may not be well suited to imperative operations like read buffers from a stream and
write buffers to a stateful database. I could be totally wrong or ignorant and would like
to one day address my ignorance.

# Making a choice

In the end Python, Ruby, Java, Go and Rust are the contenders, which probably doesn't come
as a surprise to anyone. Perhaps what surprised me is how high C rates all things considered.

I originally thought I should discount python and ruby outright, but they rank quite
highly in the scheme of things. In fact there are a few tools like [bup](https://github.com/bup/bup) written in a mix
of C and python quite successfully. I am ruling these languages out just in the off chance
that I can out perform existing software as a differentiator.

With regard to java, I feel irrational, but simply don't enjoy using it or the 
programming styles it encourages, so I am just going to veto it. 

My gut feeling is that with rust you accept pain to get performance. Go code looks
more minimal, the tools are simpler, and the language is easier to learn, but you will
always question if you are getting the best of the best or just 'good enough'.
In the end we would probably regret aspects of either choice.

This time I choose ... [rust](https://www.rust-lang.org).
