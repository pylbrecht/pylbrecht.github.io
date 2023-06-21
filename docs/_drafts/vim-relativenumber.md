---
title: "vim: j k on steroids"
layout: post
tags: vim relativenumber
---

**TLDR:**
Vim's `relativenumber` option is awesome and made me step up my Vim game by a
lot.

The `relativenumber` option is by far the one Vim feature that improved my
efficiency in day to day editting the most so far[^1] .
Moving lines with `j` and `k` always felt a little quirky.  But that ended the
moment I stumbled across this feature in a Vim talk.  The talk wasn't even
about this specific feature. The presenter just had it enabled when he was
showing some of his examples in Vim.  Someone from the audience finally asked
what the hell was going on with his line numbers. He briefly explained it and I
was instantly blown away.

[^1]: "so far", because Vim won't stop surprising me with new ways of solving particular problems more efficiently.

Since then `relativenumber` makes me smile almost every time I use it.

From the Vim help (`:help relativenumber`):
>	Show the line number relative to the line with the cursor in front of
>	each line. Relative line numbers help you use the |count| you can
>	precede some vertical motion commands (e.g. j k + -) with, without
>	having to calculate it yourself. Especially useful in combination with
>	other commands (e.g. y d c < > gq gw =).

`relativenumber` makes jumping to specific lines on the current page feel
really natural to me.
