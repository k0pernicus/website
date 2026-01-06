---
author: "Antonin"
title: "The Influentists"
date: 2026-01-06T16:00:00+01:00
draft: false
description: "Why we are losing technical rigor to social hype"
tags: ["thoughts"]
readTime: true
---

Last week, the developer community was busy discussing about a single tweet:

{{< x user="rakyll" id="2007239758158975130" >}}

The author is Jaana Dogan (known as [Rakyll](https://github.com/rakyll)), a highly respected figure in the Google ecosystem, in the open-source world, and in my heart (thank you Rakyll for your great [Go blog posts](https://rakyll.org/)).

At first glance, the tweet suggests an enormous shift in the software industry: the ability to build **in just one hour** what previously required weeks or months for a team of sofware engineers, using **just** the description of the problem. The tweet was too-much dramatic in my own opinion, but actually impressive!

The post triggered an immediate wave of "doom-posting," with many fearing for the future of software engineering (as each week since a year now).
However, as the conversation reached a high number of replies and citations on social networks, Rakyll released a follow-up thread to provide context:

{{< x user="rakyll" id="2007659740126761033">}}

This response thread revealed a story **far less miraculous** than the original tweet suggested.
Let's analyze it.

Crucially, **the foundational "thinking" had already been performed by Rakyll herself**, who guided the AI using architectural concepts (honed over several weeks or months of prior effort) rather than the AI thinking and inventing the "product" from scratch.  
Furthermore, **the resulting project was strictly a proof-of-concept** that falls far short of a production-ready system capable of managing real-world complexity.  
And finally, this success hinged on the Rakyll's **implicit domain knowledge and deep expertise**. The last point is often (strategically?) omitted from these "magic" viral demonstrations in order to make the tool appear way more autonomous than it truly is.

Hmm. Now, this is far less exciting...

## Under influence

This pattern of "_hype first and context later_" is actually part of a growing trend.  

I call the individuals participating to that trend "**The Influentists**".
Those people are members of a scientific or technical community, and leverage their large audiences to propagate claims that are, at best, unproven and, at worst, _intentionally_ misleading.

But how can we spot them?

I personally identify these "Influentists" by four personality traits that characterize their public discourse.  
The first is a reliance on **"_trust-me-bro_" culture**, where anecdotal experiences are framed as universal, objective truths to generate hype. This is a sentiment perfectly captured by the _"I’m not joking and this isn’t funny"_ tone of Rakyll’s original tweet, but also the dramatic _"I've never felt that much behind as a programmer"_ from Andrej Karpathy's tweet.
This is supported by an **absence of reproducible proof**, as these individuals rarely share the code, data, or methodology behind their viral "wins", an omission made easier than ever in the current LLM era.
And finally, they utilize **strategic ambiguity**, carefully wording their claims with enough vagueness to pivot toward a "clarification" if the technical community challenges their accuracy.

{{< x user="karpathy" id="2004607146781278521" >}}

## A growing pattern

Rakyll is far from alone. We see this "hype-first" approach across major AI firms like Anthropic, OpenAI, or Microsoft.

Consider Galen Hunt, a Distinguished Engineer at Microsoft. He recently made waves by claiming a goal to rewrite Microsoft’s massive C/C++ codebases into Rust by 2030 using AI.

![Galen Hunt original linkedin post](/images/galen_hunt_original_post.png)

When the industry pointed out the near-impossible complexity of this task, but also asking clarity for popular and critical products like Microsoft Windows, he was forced to clarify that it was only a "research project".

![Galen Hunt updated linkedin post](/images/galen_hunt_updated_post.png)

Similarly, engineers from Anthropic and OpenAI oftenly post teasers about "AGI being achieved internally" to release months later models that disappoint the crowd.

{{< x user="Simeon_Cps" id="1706078819617063304" >}}

{{< x user="ciphergoth" id="2006446942453387675" >}}

Similarly, many other companies lie over what they are solving or willing to solve:

{{< youtube id="tNmgmwEtoWE" autoplay="false" >}}

## The cost of unchecked influence

When leaders at major labs propagate these hyped-based results, it can create a "technical debt of expectations" for the rest of us. 
Junior developers see these viral threads and feel they are failing because they can't reproduce a year of work in an hour, not realizing the "magic" was actually a highly-curated prototype guided by a decade of hidden expertise.  

We must stop granting automatic authority to those who rely on hype, or vibes, rather than evidence.  
If a tool or methodology were truly as revolutionary as claimed, then it wouldn't need a viral thread to prove its worth 
because **the results would speak for themselves**.

The tech community must shift its admiration back toward **reproducible results** and away from this "_trust-me-bro_" culture.
