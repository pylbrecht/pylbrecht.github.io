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
(dare ask my wife about it), this post is all about what I learned by
contributing to Servo over the last year.

# Leveraging your tools
While working on some code implementing [`CanvasRenderingContext2D.arc()`]()
I wanted to run all
web-platform-tests[^1][^2], which make use
of this method to see if I broke anything with my introduced changes. After some
thinking I found a solution, which was embaressingly simple in retrospection:
```sh
$ find servo/tests/wpt/web-platform-tests/2dcontext/ -name "*arc*.html" | xargs -o ./mach test-wpt --debug-build --headless
```

This command basically breaks down to..
1. finding the relevant test files and..
2. executing them with `./mach test-wpt` command

I'm using the `--headless` option here, because I'm
running the tests on a remote machine over [SSH](https://en.wikipedia.org/wiki/Secure_Shell).
`xargs` requires the `-o` option, so `./mach` is content with accepting the input
from a pipe (thanks to [@nox](https://github.com/nox) for pointing this out).

Let's say we want to run only tests
related to `arc()`, which are currently failing. This way we can check if our
changes now pass some previously failing tests:
```sh
$ find tests/wpt/metadata/2dcontext -name "*arc*.html.ini" | sed 's#metadata#web-platform-tests#g;s#.ini##g' | xargs -o ./mach test-wpt --debug-build --headless
```

Again the first step is to find the relevant test files. So how do find the test
files that are currently expected to fail?

Information about test expectations are stored in `tests/wpt/metadata/` in
the form of `.ini` files. The content of such an `.ini` file could look like this:
```
[2d.drawImage.svg.html]
  type: testharness
  [drawImage() of an SVG image]
    expected: FAIL
```
In this example the test case `[drawImage() of an SVG image]` defined in the file
`2d.drawImage.svg.html` is expected to fail. Possible test
expectations are `FAIL`, `TIMEOUT` or `PASS`. If there is no corresponding
`.ini` file, the test is expected to pass. So we can assume, a present `.ini` file
means the corresponding test file contains at least one failing test case.

Since metadata files have the same name as their corresponding test files with
just `.ini` appended to them, we now have a list of test filenames we want to run.
Well.. sort of. We still have to find the *actual* test files corresponding to
the `.ini` files, so we can pass their full paths to `./mach`.

Turns out this is not that hard, because the directory structure is the same
for metadata and actual test files:
<pre>
servo/tests/wpt/<b>metadata</b>/2dcontext/drawing-images-to-the-canvas/2d.drawImage.svg.html<b>.ini</b>
</pre>
corresponds to:
<pre>
servo/tests/wpt/<b>web-platform-tests</b>/2dcontext/drawing-images-to-the-canvas/2d.drawImage.svg.html
</pre>

So we just have to replace `metadata` with `web-platform-tests` and remove the
trailing `.ini` and we're done! The `sed` part takes care of that. Since paths
contain `/`, I chose to use `#` as a separator in the `sed` expression.
Also I packed two replacements into a single expression, separated by `;`.

To me figuring out these commands was one of those "aha" moments, which
confirmed the UNIX commandline propaganda I came across on various blog posts.
A tiny step for a neckbeard, but a huge one for me actually applying the [UNIX
philosophy](https://en.wikipedia.org/wiki/Unix_philosophy) to solve a problem I
encountered while working on a real task.

[^1]: [The web-platform-tests Project](https://github.com/web-platform-tests/wpt)
[^2]: [Servo's README on wpt](https://github.com/servo/servo/blob/master/tests/wpt/README.md)

# Providing meaningful context
