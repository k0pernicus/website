+++
author = "Antonin"
date = "2016-05-19T01:32:21-04:00"
description = "A post to better understand how to use list comprehension in Nim"
tags = ["nim", "programming", "list comprehension"]
title = "List comprehension in Nim"
disqus = false
+++

[Nim](http://nim-lang.org) is a powerful programming language, developed by a strong and skillful community.
Since [my post on Twitter](https://twitter.com/k0pernicus/status/732358457567780864) 2 days ago, I wrote between 2 and 4 hours a day to program in Nim, and it's wonderful!

I love Python - especially since I discovered the book ["Fluent Python"](http://shop.oreilly.com/product/0636920032519.do) <span class="a-size-small a-color-secondary">by </span><span class="a-size-small a-color-secondary">[Luciano Ramalho](https://github.com/ramalho).
Python is a great programming language when you understand how it works, and some useful tricks like [list comprehensions](http://www.secnetix.de/olli/Python/list_comprehensions.hawk).</span> Nim inherited the syntax of Python and the performance of C.

One of the first thing I do with Nim is to build lists like Python, and I remember a lot of posts in the Internet of Nim users that don't understand how to use list comprehensions with this programming language. However, list comprehensions in Nim has basically the same implementation as Python...
This post is to give some examples of how to use list comprehensions in Nim, and make the parallel with Python to show you that it's just a "syntax thing".

To use list comprehensions in Nim, you have to import the module "future". This module contains some experimental features, which may soon be moved to core modules (Nim isn't stable for now). This is the syntax of the list comprehension macro:

<pre><span class="Keyword">macro</span> <span class="Identifier">`[]`</span><span class="Other">(</span><span class="Identifier">lc</span><span class="Other">:</span> <span class="Identifier">ListComprehension</span><span class="Other">;</span> <span class="Identifier">comp</span><span class="Other">,</span> <span class="Identifier">typ</span><span class="Other">:</span> <span class="Identifier">expr</span><span class="Other">)</span><span class="Other">:</span> <span class="Identifier">expr</span>.</pre>

A macro, in Nim, is a compile-time code transformation. So, instead of code some boring and excessive stuff to build a simple list, you just can use a flexible syntax for this. This macro takes as parameters an **expression** to build the list, and the type of elements in the list. For example, you can use this example to build a simple list of integers between 0 and 100:

<pre>import future
let my_list = lc[x | (x <- 0..100), int]
</pre>

In Python, you can program this thing using:

<pre>my_list = [x for x in range(100)]
</pre>

Ok, this example was not relevant and useful, because you can just build a generator of integers between 0 and 100 and not store all those data in a list... Let's try an other thing!

<pre>let fst_integers = [0,1,2,3,4,5,6,7,8,9,10]
let snd_integers = [30,31,32,33,34,35,36,37,38,39,40]
</pre>

Here, you have two arrays of integers: `fst_integers` and `snd_integers`, and you just want to multiply each integer of the first array with each element of the second array. It's really simple with Nim:

<pre>import future
let all = lc[(x * y) | (x <- fst_integers, y <- snd_integers), int].
</pre>

This solution outputs the expected result: `@[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 60, 62, 64, ..., 300, 310, 320, 330, 340, 350, 360, 370, 380, 390, 400]`. Now, we want to multiply each element which is a multiple of 2, from `fst_integers` and from `snd_integers`! To do that, we just have to  add a simple condition at our expression to get all multiple of 2:

<pre>import future
let all = lc[(x * y) | (x <- fst_integers, y <- snd_integers, x mod 2 == 0 and y mod 2 == 0), int].
</pre>

In the previous example, we selected interesting integers to resolve our problem: all multiple of 2\. To select a multiple of 2, we just have to know if this integer is divisible with 2! :-) We can also program the same list with:

<pre>import future
let all = lc[(x * y) | (x <- fst_integers, y <- snd_integers, x mod 2 == 0, y mod 2 == 0), int].</pre>

With Python:

<pre>fst_integers = list(range(11))
snd_integers = list(range(30,41))
all = [x * y for x in fst_integers for y in snd_integers if (x % 2 == 0 and y % 2 == 0)]</pre>

Now, what if I gave you this piece of code and asked you the solution:

<pre>import future
let d = lc[ (x,y,z) | (x <- 1..10, y <- 1..10, z <- 1..10), tuple[a,b,c:int] ]
</pre>

? This morning, a programer told me: "Easy, it's the same integer each time: (0,0,0), (1,1,1), (2,2,2), ...". NO! This code is similar to the previous one (using 2 lists). In Python, this is the implementation of the last list building:

<pre>[(a,b,c) for a in range(10) for b in range(10) for c in range(10)]
</pre>

and this code returns to you the set of each tuple existing from these initialized values! The conclusion? List comprehensions in Nim is similar to Python, and the difference is just a "syntax thing"...
