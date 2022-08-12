+++
title = "How to publish a bad programming article"
date = 2022-08-09T23:48:00+02:00
draft = false
categories = ["thoughts", "programming"]
tags = ["thoughts", "programming"]
description = "Short answer: laziness"
disqus = false
+++

***[10th of August 2022 - Article Update]***  
A friend of mine told me he would not used the word "instructions" talking 
about the size of a binary program, as there are not only instructions but the ELF structure, the imports, etc.
After its review, I changed the word "instructions" to "lines" (even if I don't really like the word "lines"...).  
Thanks Eric for the review! :)

After going to [famous orange website](https://news.ycombinator.com) I noticed an article claiming that '[“Hello world” is slower in C++ than in C (Linux)](https://lemire.me/blog/2022/08/09/hello-world-is-slower-in-c-than-in-c-linux/)'.

I was intrigued by the title at first but, after reading the first two paragraphs, I became mortified.

### The issue with the article

C++ is a **programming language** that includes its own standard library, but can also work with the
 C standard library.
When you make comparisons, and especially if you publish the results publicly, you have to make sure 
that what you are comparing can actually be compared (in short: do not compare apples with oranges).

In this article, the author compares two different programs.  
A C program:
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("hello world\n");
    return EXIT_SUCCESS;
}
```

and a C++ program:
```cpp
#include <iostream>
#include <stdlib.h>

int main() {
    std::cout << "hello world" << std::endl;
    return EXIT_SUCCESS;
}
```

However, as you already noticed, those programs are valid C and C++ programs but **those programs compare the `stdio` module against the `iostream` module**.

**Note**: `std::endl` does a buffer flush before printing the message on the standard output compared to the C version, 
so the code is not strictly equivalent to the C sample...

### Let's redo the experiment

If we want to compare both languages, we actually have to compare the binaries (size / memory performance / execution time / ...)
of the same program compiled by different compilers: a C compiler, and a C++ compiler.  
So, if we really want to compare the languages, lets take only one code sample (the C sample), compiled with **the same**
parameters, lets run them at least 5_000 times x 10 times (to avoid some computer working behaviours) and extract the 
median of those bench (and the standard deviation as well, always interesting to take a look at this).  

**Note**: Also, the author did not explained which version of gcc/g++/clang, and which version of c++, to use to **reproduce** the 
results, which is a bad sign...

After updating the Makefile:
```makefile
all: helloc hellocpp
	hyperfine --runs 5000 ./helloc
	hyperfine --runs 5000 ./hellocpp

helloc: hello.c
	clang -O2 -o helloc hello.c -Wall

hellocpp: hello.cpp
	clang -O2 -std=c++17 -o hellocpp hello.cpp -Wall
```

Here, the .cpp and the .c files are stricly equivalent:

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("hello world\n");
    return EXIT_SUCCESS;
}
```

I used `clang` version 13.1.6 for this experiment.

After compiling the binaries, lets compare the size first:
```sh
> wc -c < helloc
   33427
> wc -c < hellocpp
   33429
```

The C binary has 33427 lines, and the Cpp binary 33429 lines... quite a lot to print an "hello world"!  
So, both binaries are different, as the cpp output has 2 lines more than the C one (could be instructions, a header 
difference in the ELF structure, etc...).

And how many lines differ?

```sh
> cmp -l helloc hellocpp | wc -l
   376
```

376 lines are different between both binaries, which is 1.12% of the entire size of the cpp binary (not so much).

### The issue with hyperfine

`hyperfine` is a correct tool to make basic benchmarks but, as every software in the market, its performance and results
depend of the conditions during the tests.
To demonstrate this, I launched `hyperfine` 5000 times for the same C program.
While `hyperfine` was running the C program, I opened my text editor and began to write this article, knowing 
that it should reduce drastically (all is relative) the performance running this simple C program.  
For the C++ one, I let `hyperfine` running without touching the computer, and... the results are amazing, as
**C++ is now three times faster than C**!

```sh
> hyperfine --runs 10000 ./helloc
   Benchmark 1: ./helloc
   Time (mean ± σ):      10.7 ms ±   6.0 ms    [User: 1.5 ms, System: 3.0 ms]
   Range (min … max):     0.0 ms … 193.3 ms    10000 runs

> hyperfine --runs 10000 ./hellocpp
   Benchmark 1: ./hellocpp
   Time (mean ± σ):       3.4 ms ±   6.8 ms    [User: 0.4 ms, System: 0.8 ms]
   Range (min … max):     0.0 ms … 424.8 ms    10000 runs
```

This is, of course, an example of "**what you should absolutely avoid**", as the conditions to test the first
and the second programs were not the same.
And, if comparison conditions are not the same, then you can't compare the results...

Also, this is to demonstrate why you should run multiple times your benchmarks, with no software running in the background (especially for very little programs like those).

### Running the real tests

Running the same both programs with the same conditions **only ten times**, this is the median of the observations:
* C: **3.2ms**, standard deviation of **0.8ms**,
* C++ (with c++17): **3.2ms**, standard deviation of **0.9ms**.

**Same thing.**

### Conclusion

The author stated in its blog post:
> What we are testing by running these programs is the overhead due to the choice of programming language.
> [...]
> One millisecond of overhead, if it is indeed correct, is a huge penalty.

**This is wrong**, as the author measured the overhead due to the choice of API used to print to the standard 
output, and not the overhead of running the same program compiled with a C compiler, and another with the C++ 
compiler.

**Final Note**: to finish, the author just demonstrated that `iostream` is slower than `printf`, and I think every
 good or curious C++ developer in the world already knew that...
