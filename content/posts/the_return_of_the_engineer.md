---
author: "Antonin"
title: "By necessity: the return of the engineer"
date: "2026-06-12T23:15:00+02:00"
tags: ["thoughts"]
description: "The future of engineering interviews"
toc: false
readTime: true
draft: false
---

Programming tests used to be our primary filter to separate profiles.

In software engineering, they historically differentiated three distinct ones:

- the **engineers**, the ones who solve the fundamental problems.
- the **developers**, the ones who apply the right patterns and orchestrate the architecture depending on the needs.
- the **coders**, the ones who just write code, code, and code.

We used to put candidates through exhausting steps to check everything: their writing, their communication, their "syntax recall", and if their mind fit the culture of the team.

Before the LLM boom, the live coding interview was already questionable as, most of time, it was a "performance of memorization".
However, today, it became pointless.

## Shifting

Culture fit remains the immovable constant. A toxic genius or an uncommunicative teammate is still a catastrophic hire, regardless of the tools they use. And, as you might alredy know, AI doesn't fix human behaviours.

But the coding test itself **is dead**, not because writing code isn't useful (I will always be the one who considers that making a thing is the best way to understand that thing), but because **cheating the test is easier than ever**. We are entering an era where a Builder agent can generate a 500-line pull request in a matter of seconds, mostly slop, and submit it directly in an ocean of slops. 
In order to improve the workflow, the primary job of the human is shifting. Some of us are no longer just creators, but (unfortunately, in my own opinion) editors and auditors. 

## The five new pillars

If we strip away the obsolete rituals, we are left with a new reality.  
In order to reflect this reality, I personally would replace the traditional tests with five distinct checks, or pillars:

- **the cultural fit check**.
- **the algorithmic core**: if you can’t design an algorithm with nothing but pseudo-code, you will write unscalable, unmaintainable software. Worse: you will let an LLM write it for you, and you won’t even get the **intuition** to realize it is fundamentally flawed.
- **the architectural feeling**: the hardest problems today are not about writing isolated functions but managing data flow, handling asynchronous events, defining strict API contracts, and surviving the chaos of distributed architectures. This is still a valuable skill (which is even [not even owned at Anthropic apparently](https://status.claude.com/)...).
- **the code review**: if you can’t review code, you can’t direct AI tools. You cannot lead a team. You cannot even trust your own work.
- **debugging code**: if LLMs are doing the heavy lifting of writing the boilerplate, the human is left with the hardest part: debugging. A great modern interview test would hand a candidate a codebase riddled with subtle, AI-generated logic flaws and ask them to find and fix the bugs.

**At the end, if a candidate cannot critique, debug, and verify code, they are not an asset but a liability.**

## The Mud and the Mind

Previously, we had room for all three profiles, and our tests were designed to expose those specific skills.

**Today, there is a catch.**  
The trend is undeniable: LLMs are the ultimate *coders*. They produce boilerplate infinitely, instantly, without fatigue. So, we are leaving the coders behind, returning, **by necessity**, to a future built entirely by engineers.  
However, because our day-to-day work is shifting toward auditing and reviewing, a dangerous corporate fantasy is taking root: the belief that engineers should simply become project managers.
Let us not confuse this evolution with a retreat. **Becoming an auditor does not mean keeping your hands clean**.

This is a profound misunderstanding of the craft. An engineer’s primary output has never been code but always been **deep thought** on how to solve efficiently a problem.
To solve a problem, you will need *intuition*.  
To develop intuition you need *experience*. 
And to gain *experience* you definitely need to **put your hands in the mud**.  
You still need to feel the raw, unabstracted reality of the systems you deploy. You have to break your teeth on the walls of complex architecture to actually develop intuition.

Companies that confuse engineering with mere ticket-shuffling will inevitably build fragile, incomprehensible black boxes.
If an organization expects you to be a PM rather than a deep technical thinker, they have already lost the plot.  
In this special case I have an advice: **ignore them, and solve problems elsewhere**.
