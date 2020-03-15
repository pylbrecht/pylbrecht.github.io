---
title: "Learning on steroids"
layout: post
---

Bored at your job?
The same tools and technologies day in, day out?
Bring excitement to your life!
Contribute to the [Servo Project](https://servo.org/)! Pick your [first
issue](https://starters.servo.org/) now and receive a rewarding challenge of
fixing interesting bugs or implementing new features! And here's the kicker:
it's completely free!

Ok, enough with the sales pitch. I guess you can tell already how much I
fell in love with contributing to Servo. Almost one year ago I started to
consistently work on Servo issues in my spare time and besides learning Rust
and having tons of fun, it taught me to become a better developer in so many
ways. Mostly because the Servo team members are truly amazing people, who
openly share their passion for technicalities with newcomers and make the
overall contributing experience very pleasant, intriguing and potentially
addictive. But as much as I love to brag about how amazing Mozillians are
(dare asking my wife about it), this post is all about what I learned by
contributing to Servo over the last year.

# Leveraging your tools
While working on [some code](#issue/PR) implementing [`CanvasRenderingContext2D.arc()`]()
I wanted to run all
[web-platform-tests](https://github.com/web-platform-tests/wpt), which make use
of this method to see if I broke anything with my introduced changes. After some
thinking I found a solution, which was embaressingly simple in retrospection:
```sh
$ find servo/tests/wpt/web-platform-tests/2dcontext/ -name "*arc*.html" | xargs -o ./mach test-wpt --debug-build --headless
```
To me this was one of those "aha" moments, which confirmed the UNIX commandline
propaganda I came across on various blog posts. I know.. a tiny step for a
neckbeard, but a huge one for me actually applying the [UNIX philosophy](https://en.wikipedia.org/wiki/Unix_philosophy)
to solve a problem I encountered while working on a real task.

I'm not explaining the details of this command, since it shouldn't be too hard
to figure it out by studying the corresponding [man
pages](https://en.wikipedia.org/wiki/Man_page)
and asking your favorite search engine (hint: the `-o` option of `xargs` was
the hardest part, because `./mach` wasn't very content getting stuff piped to it).

# Providing meaningful context

