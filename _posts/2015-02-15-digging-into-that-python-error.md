---
title: 'Digging into &#8220;that&#8221; Python error'
author: James
layout: post
permalink: /digging-into-that-python-error-1268/
pw_single_layout:
  - 1
dsq_thread_id:
  - 3518893716
categories:
  - Articles
tags:
  - deep-dive
  - python
---
I think this is my most popular tweet ever:

<blockquote class="twitter-tweet" lang="en">
  <p>
    >>> foo = ([],)&#10;>>> foo[0] += [1]&#10;TypeError: 'tuple' object does not support item assignment&#10;>>> foo&#10;<<< ([1],)
  </p>
  
  <p>
    &mdash; James Socol (@<a href="http://twitter.com/jamessocol">jamessocol</a>) <a href="https://twitter.com/jamessocol/status/565906946780581889">February 12, 2015</a>
  </p>
</blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>


I&#8217;ve known about this little quirk for a while, but I shared it because it still amuses me. It shocks and confuses people. Some people tried to make sense of it

<blockquote class="twitter-tweet" data-conversation="none" lang="en">
  <p>
    <a href="https://twitter.com/jamessocol">@jamessocol</a> Like this:&#10;i=([],)&#10;f=i[0]&#10;f+=[1]&#10;i&#10;([1],)&#10;Surely that's equivalent, & it works. Like you said, corner case.
  </p>
  
  <p>
    &mdash; Ramon Ferrandis (@<a href="http://twitter.com/ramoncreager">ramoncreager</a>) <a href="https://twitter.com/ramoncreager/status/566692839598592001">February 14, 2015</a>
  </p>
</blockquote>

And some were just appalled

<blockquote class="twitter-tweet" data-conversation="none" lang="en">
  <p>
    <a href="https://twitter.com/jamessocol">@jamessocol</a> Thanks for sharing. This is the first Python quirk which makes me think of PHP/JavaScript.
  </p>
  
  <p>
    &mdash; Eason Chen (@<a href="http://twitter.com/easoncxz">easoncxz</a>) <a href="https://twitter.com/easoncxz/status/566719670414503936">February 14, 2015</a>
  </p>
</blockquote>

So what actually *is* going on here? To answer, we need to take a dive into the world of CPython *opcodes* and spend some time with the [`dis` module][1].

Let&#8217;s start by disassembling and digging into a very simple set of statements:

<script src="https://gist.github.com/jsocol/1c1912c755512c41fc61.js?file=1f.py"></script>

And the output:

<script src="https://gist.github.com/jsocol/1c1912c755512c41fc61.js?file=1o.dis"></script>

The left-most number is the line number (line 1 is `def f1():`) then an offset we&#8217;ll ignore, then the opcode, then some information about the arguments to the opcode.

All of these opcodes operate on &#8220;the stack,&#8221; and most often on the value on the top of the stack, `TOS`. We load values onto the stack from constants, functions, and in-scope variables, and we can store values into in-scope variables. Sometimes we&#8217;ll deal with a couple of values at once, which we&#8217;ll call `TOSn`, where `n` is the depth in the stack. Since the stack is, after all, a stack, we can&#8217;t operate on `TOS1` unless we&#8217;re also operating on `TOS`.

Line 2 has two operations: `LOAD_CONST`, which pushes the constant `` onto the stack. Then `STORE_FAST` takes the value off the top of the stack and stores it in the local variable `a`, which is a human name for variable number ``.

Line 3 is a little more interesting: first we load the value from `a` onto the stack (`LOAD_FAST`), then load the constant `1` on top of it (`LOAD_CONST` again). The `INPLACE_ADD` operation pops the top two stack values, executes `__iadd__`, and pushes the result back onto the stack. Then `STORE_FAST` takes the value off the top of the stack and stores it back into `a`.

The last two opcodes are the implicit `return None` from this function, so we&#8217;ll ignore those.

There are a couple of interesting things to note here: one is that `INPLACE_ADD` is actually less &#8220;in-place&#8221; than we were lead to believe. It still loads the value onto the stack and then stores a new value. It is not single atomic operation, but rather 4 operations. Another is to recall that number types, like strings and tuples, are *immutable*.

Immutability is going to be very important, so keep that in mind. There is no integer method that changes the value of an integer. Any time we operate on an integer value, we need to store the result again. `__iadd__` returns a new value that *replaces* the current one, it doesn&#8217;t *change* the current one. There is no unary increment (`++`) operator in Python.

Let&#8217;s change `f1` slightly and see what happens:

<script src="https://gist.github.com/jsocol/1c1912c755512c41fc61.js?file=2f.py"></script>

<script src="https://gist.github.com/jsocol/1c1912c755512c41fc61.js?file=2o.dis"></script>

Line 2 is the same: push `` onto the stack, then pop it and store it in local variable 0/`a`.

Line 3 starts off the same: we still load `a` and push it onto the stack, then load the constant 1 and push it onto the stack. But this time we call `BINARY_ADD` which pops the top two values, adds them with `__add__`, and pushing the result. Then `STORE_FAST` pops and stores it in `a`. Finally the implicit return we&#8217;ll ignore.

OK, integers are fun, but what about a more interesting type, like a list?

<script src="https://gist.github.com/jsocol/1c1912c755512c41fc61.js?file=3f.py"></script>

<script src="https://gist.github.com/jsocol/1c1912c755512c41fc61.js?file=3o.dis"></script>

Start with the LHS of line 2: `BUILD_LIST 0` takes zero objects off the top of the stack, builds a list object, and pushes that list onto the stack. Then we `STORE_FAST` which puts that list into `a`.

On line 3, because we&#8217;re doing an &#8220;inplace&#8221; add again, we follow the same road map: `LOAD_FAST` pushes the value from the variable stored on the RHS back onto the stack (the list). Then we evaluate the LHS: `LOAD_CONST` to push `1` onto the stack, call `BUILD_LIST 1` to create a list containing the top value from the list, and push the new list onto the stack. Then call `INPLACE_ADD` which, for lists, executes the extend operation and pushes the new list onto the stack. Finally, pop the new list and store it in our local variable.

Now let&#8217;s compare that with calling `.extend()`:

<script src="https://gist.github.com/jsocol/1c1912c755512c41fc61.js?file=4f.py"></script>

<script src="https://gist.github.com/jsocol/1c1912c755512c41fc61.js?file=4o.dis"></script>

On line 3, we push the list from `a` onto the stack, then call `LOAD_ATTR` to pop it off the stack, and then push a reference to the `extend` attribute onto the stack. `LOAD_CONST` to push `1` onto stack, `BUILD_LIST` replaces it with a list `[1]` then `CALL_FUNCTION 1` which pops 1 value from the stack to use as a function argument, then pops the function, and finally calls the function, which, in this case, extends the list with that value, then pushes the result onto the stack.

What happens next is interesting. We do *not* call `STORE_FAST`. Lists are *mutable*, and the value of the list has already changed. We just call `POP_TOP` to throw away the top stack value.

At this point, we can basically understand how this bizarre error both works and fails at the &#8220;same time,&#8221; but there&#8217;s one more type of operation we should cover.

<script src="https://gist.github.com/jsocol/1c1912c755512c41fc61.js?file=5f.py"></script>

<script src="https://gist.github.com/jsocol/1c1912c755512c41fc61.js?file=5o.dis"></script>

Line 2 builds a list just like we&#8217;ve been doing: `LOAD_CONST` to put `5` on the stack, `BUILD_LIST 1` to build a list with 1 value from the stack, and `STORE_FAST` to pop the list from the top of the stack and store it into `a`.

Line 3 `LOAD_FAST`s the list onto the stack, then `LOAD_CONST` the index (0) from the LHS onto the stack. `DUP_TOPX 2` takes the top two values from stack and duplicates them, in order, which makes the stack `a 0 a 0`.

`BINARY_SUBSCR` takes the top two values from the stack, evaluates `TOS1[TOS]`, and pushes it back onto the stack, so the stack is now `a 0 5`. `LOAD_CONST` puts the value `1` onto the stack, and `INPLACE_ADD` pops the two integers, adds, then, and pushes `6` back (`a 0 6`).

`ROT_THREE` rotates the top three stack objects, so we reorder the stack to `6 a 0`. We need to do this to call `STORE_SUBSCR`, which implements `TOS1[TOS] = TOS2`, or `a[0] = 6`. `STORE_SUBSCR` replaces `STORE_FAST` in the other examples, and it changes the stored value of `TOS1`, so it only makes sense for *mutable* types.

OK! Finally, we&#8217;ve got everything we need to look at the our favorite bizarre behavior:

<script src="https://gist.github.com/jsocol/1c1912c755512c41fc61.js?file=6f.py"></script>

<script src="https://gist.github.com/jsocol/1c1912c755512c41fc61.js?file=6o.dis"></script>

Line 2: build a list and push it. (`[]`) Pop it and use it to build a tuple, then push the tuple (`([],)`) Pop the tuple and store it.

Line 3:

  * `LOAD_FAST` Push `a` onto the stack (`([],)`)
  * `LOAD_CONST` Push `` onto the stack (`([],) 0`)
  * `DUP_TOPX 2` Duplicate the two stack values (`([],) 0 ([],) 0`)
  * `BINARY_SUBSCR` Pop 2, calculate `TOS1[TOS]`, push the result (`([],) 0 []` Remember that the list inside the tuple and the list are the *same object*.)
  * `LOAD_CONST` Push `1` onto the stack (`([],) 0 [] 1`)
  * `BUILD_LIST 1` Pop the top, build `[TOS]`, and push the result (`([],) 0 [] [1]`)
  * `INPLACE_ADD` Pop the top two, execute `TOS1.__iadd__(TOS)`, extending the list (`([1],) 0 [1]`) At this point *the extend operation has already happened*.
  * `ROT_THREE` (`[1] ([1],) 0`)
  * `STORE_SUBSTR` Attempt to pop the top three and store `TOS1[TOS] = TOS2`.

`STORE_SUBSTR` fails because tuples are immutable, which causes the `TypeError`. But, as we learned, `+=` is *not* an atomic operation. Part of it (the actual addition/extend) can succeed while another part (store) fails.

Without using `dis.dis`, can we tell how Ramon&#8217;s example is different?

  * `i = ([],)` builds a list, pushes it onto the stack, pops it to build a tuple, pushes the tuple, and finally pops the tuple stores it in the local variable `i`.
  * `f = i[0]` is going to do something like `LOAD_FAST` to get the tuple onto the stack, `LOAD_CONST` to get `` onto the stack, `BINARY_SUBSCR` to pop them and push `TOS1[TOS]`, a reference to the list, onto the stack, then `STORE_FAST` to pop it and store the reference in local variable `f`.
  * `f += [1]` will `LOAD_FAST` the list from the RHS, `LOAD_CONST` to push `1` onto the stack, `BUILD_LIST 1` to pop `1` onto the stack, build a list, and push it onto the stack. `INPLACE_ADD` to pop the top two, add (extend) them, and push the result onto the stack, and finally `STORE_FAST` to pop the extended list into `f`.

Since `f` and `i[0]` point to the same list object, `i` is now `([1],)`. `i` hasn&#8217;t mutated but its mutable member has, and nothing ever tried to do a `STORE_SUBSCR` on an immutable object.

This is specifically CPython (I used 2.7.5) and the opcode representation is not guaranteed or part of the spec. It&#8217;s internal and subject to change, but it&#8217;s also an interesting introduction to an important aspect of how Python works.

The original, specific example is contrived and bad code, specifically constructed to fail. But it&#8217;s not *so* bad or *so* contrived that no one would ever do it, which is why it&#8217;s a particularly interesting one.

 [1]: https://docs.python.org/2/library/dis.html
