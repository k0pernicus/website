+++
title = "The state of memory safety in Chromium"
date = 2020-05-24T00:00:00+02:00
draft = false
categories = ["chromium", "memory safety", "memory bugs"]
tags = ["chromium", "memory safety", "memory bugs"]
description = "Close the tap"
disqus = false
+++

As [Microsoft last year](https://medium.com/tadaweb/security-by-design-a-brief-introduction-to-rust-378060e45038), the Chromium project team, responsible for the Chromium browser (which is the base of many open source and proprietary projects like Chrome, Brave, or Visual Studio Code), released a blog post last week about [memory bugs in Chromium](https://www.chromium.org/Home/chromium-security/memory-safety).

## The analysis

The analysis has been based on both past and present 912 high or critical severity security issues, since 2015.  
Based on those bugs, the observation is clear:

> The Chromium project finds that around 70% of our serious security bugs are memory safety problems.  
[...] Around 70% of our high severity security bugs are memory unsafety problems (that is, mistakes with C/C++ pointers). Half of those are use-after-free bugs.

{{< figure src="/images/memory_bugs_chromium_chart.png" title="Chart about memory bugs on Chromium, from Chromium Project team" scale="50%" >}}

Despite the efforts of the project team to maintain *good* security architecture in managing [sandboxing](https://chromium.googlesource.com/chromium/src/+/master/docs/design/sandbox.md) and [site isolation](https://www.chromium.org/Home/chromium-security/site-isolation), to let those heavy components manage and contain the severity of memory bugs has both limitations and resources cost.  
Also, even if those security components (or layers) stay necessary for daily usage, [this not enough to stop attackers and avoid attacks](https://googleprojectzero.blogspot.com/2020/02/escaping-chrome-sandbox-with-ridl.html).

Instead, as stated in the blog post:

> [...]The cheapest way to maintain the advantage is to squash bugs at source instead of trying to contain them later.

## The different solutions

As always in software projects, solutions (and decisions) come with a trade-off.  
Indeed, the team developed a plan to solve those issues, using two criterias:

* time and resources costs needed to implement the improvement(s),
* the real benefit of those costs, *basically the number and the quality of those improvement(s)*.

{{< figure src="/images/memory_bugs_chromium_plan.png" title="Plan for next years memory bugs improvements for / in Chromium, from Chromium Project team" scale="50%" >}}

This spectrum reflects the different strategies offered by the team, from left to right:

1. to keep their dev model in unsafe system programming languages (like C or C++) but to bring attention on memory safety first (spatial memory safety, custom C++ libraries, and moving to newer standards like [C++17](https://en.cppreference.com/w/cpp/17) or the next C++20);  
2. customize their needs using custom C++ dialects, and / or continue on their idea of using a [C++ garbage collection library for Chromium](https://docs.google.com/document/d/1Cv2IcsiokkGc2K_5FBTDKekNzTn3iTEUyi9fDOud9wU/edit#heading=h.i5ibcxqde9h2);
3. move to a safe system programming language (and ecosystem) like [Swift](https://developer.apple.com/swift/) or [Rust](https://www.rust-lang.org/), to bind with C++ parts first before rewriting those.

This plan is not new, as [the Mozilla foundation already made experiments about switching from C++ to Rust since 2015](https://wiki.mozilla.org/Oxidation)).  
Indeed, to make the switch to another technology has a big cost in a project, because it implies both to limit new upcoming features and to give some time to developers to embrace a new technology that they may not be accustomed to use daily.
Also, the plan here is to have a long-term vision, and not switching again to another technology in five or ten years.

In my opinion, to switch first to "modern C++" can be a good option **for a short term solution**, but is [may not be a great option to "save your ass" all the time](https://alexgaynor.net/2019/04/21/modern-c++-wont-save-us/).  
Rust of Swift are definitely game changers in tech since their first stable version release.

## To go further

* An excellent blog post from Mozilla about preventing developers about memory safety and memory bugs: [link to blog article](https://hacks.mozilla.org/2019/01/fearless-security-memory-safety/).  
* My article about security by design, in the [Tadaweb](https://www.tadaweb.com) blog: [link to blog article](https://medium.com/tadaweb/security-by-design-a-brief-introduction-to-rust-378060e45038)
