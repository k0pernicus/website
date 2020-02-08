+++
title = "Zero-cost abstractions in Rust"
date = 2020-02-08T23:10:00+02:00
draft = false
categories = ["Rust", "zero-cost abstraction", "compiler"]
tags = ["Rust", "zero-cost abstraction", "programming language design", "compiler"]
description = "Abstractions before optimizations"
disqus = false
+++

Last week, [Ibrahim Dursun](https://idursun.com/) published [an article about zero-cost abstractions in Rust](https://idursun.com/posts/Rust_zero_cost_abstractions_in_action/).  
Unfortunately, except for a subpart of the article, this article did not reflect, in my own opinion, correctly what are zero-cost abstractions.

Indeed, zero-cost abstractions, or "zero-overhead", can be difficult to understand and to separate from other compiler optimizations, and can be easily misunderstood.

In this blog article, I discuss about this specific feature, and give you an example of how Rust is using it to deliver optimized code of your abstracted projects.

**Note**: an excellent blog article [about zero-cost abstractions](https://boats.gitlab.io/blog/post/zero-cost-abstractions/) already exists, written by [withoutboats](https://boats.gitlab.io/blog/page/about/), last year. My blog post can be an additional content to the article of withoutboats.

## What are zero-cost abstractions

When you are developing a project, and the project is becoming more and more complex, you generaly want to include abstractions in your code to make it easier to maintain and to customize, to add more features.
In this case, your last wish is that those abstractions penalize the performances of your program at runtime.

So, as a software developer, you generaly want two things:  

* to design your project where code readibility and code management are more important than complex hand-writtened optimizations,
* still benefits of great software performances at runtime.

Indeed, improving code readibility implies to limit code optimizations and memory overhead, which comes with indirect memory access that can cause performance issues at runtime.

Zero-cost abstractions are coming from C++, and defined by Bjarne Stroustrup in [Foundations of C++](http://www.stroustrup.com/ETAPS-corrected-draft.pdf):

> In general, C++ implementations obey the zero-overhead principle: What you don't use, you don't pay for. And further: what you do use, you couldn't hand code any better.

To summarize what Bjarne Stroustrup said, using zero-cost abstractions:

1. you don't pay the features you don't use,
2. the high levels APIs (or concepts) will compile to machine code as good as what you could get by writing more lower-level code.

This means that most of the abstractions provided by the programming language (or it's standard library) will not add any additional cost ***at runtime***.

In Rust, zero-cost abstractions is a core principle for:

* compile-time memory checks and **static** garbage collection (*borrowing* and *ownership*) - as a reminder, Rust does not check and collect memory at runtime, but is tracking the lifetimes of your code entities at compile time, instead of using reference counting or gargage collection,
* [traits](https://blog.Rust-lang.org/2015/05/11/traits.html), which is one of the most impressive feature to extend the types used by your Rust programs,
* generics,
* iterators,
* etc.

This can explain why [Diesel](https://diesel.rs/), a famour Rust ORM, [was 30% faster than the raw postgres solution for Rust](https://www.reddit.com/r/Rust/comments/5hmya5/diesel_is_30_faster_than_Rustpostgres_in/), using more abstractions.

## Example

Let's take a simple example to illustrate this feature.  
In this section, we will... compute the sum of odd numbers.

To solve this problem, we can write two functions:

```Rust
fn sum_odd_numbers(n: u64) -> u64 {
    let mut acc = 0;
    for element in 0.. {
        if element >= n {
            break;
        }
        if element.is_odd() {
            acc += element;
        }
    }
    acc
}
```

, or

```Rust
fn sum_odd_numbers(n: u64) -> u64 {
    (0..)
        .take_while(|element| element < &n)
        .filter(|n| n.is_odd())
        .fold(0, |sum, element| sum + element)
}
```

The second code sample borrows functional programming concepts to compute the sum, and is seems more "compact" (and easier to understand) than the first one.  
However, if we decompose this second sample, we can make wrong assumptions here:

1. create an iterator that begins with a zero value,
2. take all elements lower than `n`, a number passed as parameter by the user - as a reminder, if we explicit the final of the iterator, the
compiler could decide to compute the final number at compile time, and store it in the executable -, and store the result in another array,
3. loop all over the last array of elements to get only the odds, and store the result in another array,
4. loop over the odds array to compute the sum, and return finaly return the value.

So, multiple array allocations just to compute a simple sum...

Fortunately, **Rust does not do that**: instead, the compiler writes as great code as the first solution provided here, by itself!

Let's demonstrate it using benchmarks.

### Benchmarks

The following benchmarks have been made using an Intel Core i5 (3 GHz, 6 cores * 2 threads / core), and show the median execution time of the both programs, hand-optimized and abstracted, ran ten times, and for different `n` values.  
I used the `time` program to measure the user + system execution times each time, and computed the median of those values.

I performed the benchmarks for both debug and release versions.

As a reminder, to build a release version of a Rust program using `cargo`:

```bash
cargo build --release # in the root of your cargo project
```

To finish, the code repository of each code sample is available in a public github repository, at [https://github.com/k0pernicus/Rust_zero_cost_abstraction_illustration](https://github.com/k0pernicus/Rust_zero_cost_abstraction_illustration), if you want to reproduce them yourself at home.

**Benchmarks**

| Version |  n  | Hand-optimized execution time | Abstracted version execution time |
|---------|-----|-------|-------|
|  Debug  | 100 | 0,02s | 0,01s |  
|  Debug  | 100000 | 0,02s | 0,03s |  
|  Debug  | 100000000 | 2,97s | 6,59s **(x2.22)** |  
|  Debug  | 1000000000 | 29,61s | 65,16s **(x2.20)** |  
|---------|------------|--------|--------|
| Release | 100 | 0,00s | 0,00s |
| Release | 100000 | 0,00s | 0,00s |
| Release | 100000000 | 0,03s | 0,03s |
| Release | 1000000000 | 0,26s | 0,26s |

As you can observe, optimizations for the abstracted version are mostly performed for the *release* version, and both program versions have the same runtime performance in comparison with the debug versions.

## Compile time consequences

Unfortunately, zero-cost abstractions has a consequence: as the compiler has more work to do to optimize itself the abstracted code, it will run more longer.

If you take a look at the compile time of each versions below, you can observe that the abstracted version compiles more slower than the hand-written version, even for a simple program like this...

| Version | Hand-written optimized | Abstracted |
|---------|------------------------|------------|
| Debug   | 0,01s | 0,01s |
|---------|------------------------|------------|
| Release | 5,80s | 8,19s **(x1.41)** |

## Conclusion

Do not hesitate to include abstractions in your code when you need them - in Rust, you should never think about hand-written optimizations first, but think about abstractions, software design, and code readibility, **even if you care about runtime performances**.  
Unfortunaly, this has a (heavy) cost at compile time.
