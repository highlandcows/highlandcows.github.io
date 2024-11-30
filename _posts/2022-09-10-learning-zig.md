---
title: Learning Zig
date: 2022-09-10
category: coding
tags: zig journey
---

I recently stumbled across a small C-like language called
[Zig](https://www.ziglang.org). It's still under major development and is a bit
rough around the edges, but it is quite usable so long as you're willing to jump
into the source from time to time. As far as I can tell, it's largely the work
of one person, Andrew Kelley. He writes about its goals in a [blog
entry](https://andrewkelley.me/post/intro-to-zig.html) from 2016.

Early in my career, I worked almost exclusively in C. It was the first language
that excited me about coding and although it was a beast to work in, it was also
a very transparent, i.e., you pretty much always knew what a given piece of C
code was doing. Although its lack of memory management occasionally produced
crashes that were challenging to fix. I think I probably benefited from the fact
that my time with the language pre-dated multi-threaded applications - the main
mechanism for parallelism was still primarily at the process level.

Like many developers who got their start in the 1980s, I moved from C to C++ and
then onto Java.  C++, in retrospect, was a messy affair. It always had a very
"kitchen sink" feel to it which, so far as I could tell, only got worse as it
matured. Java was a breath of fresh air in the mid-1990s and I enjoyed working
in it. While Java itself has not grown as much as C++ - it's had some useful
additions like lambdas - the Java ecosystem itself has grown considerably. As
I've written [elsewhere]({% post_url 2018-03-01-scala-journey %}), I've been working primarily in
Scala since 2015.

Scala, is probably about as far from C as one can imagine. So when I found Zig,
I decided that I needed to have a go at learning it (no pun intended!). I tried
to learn Rust but found that its memory management was too complex to use on a
casual basis - I suspect that if I was using it on a daily basis I would be more
comfortable with the complexity.

Zig, on the other hand, is much closer to C in feel. The langauge is small. It
makes a nod to object-oriented programming with the ability to add methods to
`struct`'s but is not unduly O-O. It also tries to give a bit more safety to
working with references (as opposed to values) but again, does not shield the
developer from the underlying memory model. In particular, it is does not have
garbage collection, so one is back to managing memory directly.

It also has an interesting capability that I am still wrapping my brain around
which is the ability to specify that certain code is "comptime" - which is a
shorthand for "compile time". Code that uses the "comptime" moniker has access
to a number of interesting features, including that it will be inlined. However,
what I like about the "comptime" facility is that it is more structured than the
C preprocessor - the source of some of the most obfuscated code that I have ever
seen.

It is also very easy to call C code from Zig - this is intentional and
definitely a major strength of the language.

My first non-trivial bit of code in Zig was an
[implementation](https://github.com/njacobs5074/zig/tree/main/dining_philosphers)
of the Dining Philosophers' Problem. However, I would be remiss if I did not
give major kudoes to Dave Gauer, who has written an extensive tutorial in Zig,
[ziglings](https://github.com/ratfactor/ziglings). I highly recommend working
through the 90+ exercises he has written. At each step, he guides the student
through the process of fixing or tweaking code that is broken in some way. It's
a great introduction to the language.

It's not all milk and honey, though. I have had to become re-acquainted with my
old friend, SIGSEGV (segmentation violation), but as frustrating as those are,
there is also something very reassuring about its simplicity. I don't think I'll
be using Zig for my day job just yet but as they say, "Never say never."
