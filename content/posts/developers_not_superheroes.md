+++
title = "Developers are not superheroes"
date = 2020-04-25T19:00:00+02:00
draft = false
categories = ["devlife"]
tags = ["developer", "devlife"]
description = "Stop the bullshit"
disqus = false
+++

There is a myth in IT.  
The myth of the "super code monkey".

The "super code monkey" is an expert in everything, in every situation.  
The "super code monkey" does not introduce bugs when he writes code.  
The "super code monkey" sleeps at least eight hours every day.  
The "super code monkey" is young, has a social life, and practice sport each day to keep in shape.  
The "super code monkey" is an expert in Android and iOS apps development, he knows how to write excellent code in, at least, C, C++, and Java.  
The "super code monkey" studied Data Science in three weeks, and became an expert on Python, R, Matlab, Octave, and Julia in less than a month.  
The "super code monkey" has strong knowledge in Docker and Kubernetes, and masterized 10 different Javascript frameworks the last six months.

But, as every myth... the "super code monkey" does not exists!  
Except maybe in the “requirements" section of some IT recruitment companies...

In this blog post, let's discuss about the sense of the word "expert" in Computer Science, softwares without bugs (*spoiler: it does not exists*), and the
fatal comparison with known figures from an old era...

## What is an "expert" ?

Let's define what I understand by "someone is *expert* in...".

You can be "good" at something, but you **must** have a *professional* experience to prove in order to be considered as an expert.
Also, you can't speak about being an expert in a given subject without explaining the context you applied to it, and without maintaining a recurrent technological watch on the subject you are mastering.  
As an example, a data scientist can claims that he is a Python expert for Data Science, and he know really well how to use famous modules like Pandas, or Scikit-Learn. But, this is totally different for a network engineer, or for a software pentester!  
All three will be able to write very good Python code *associated to the context of the application*.

Becoming an expert on subject or technology ***X*** depends strongly of two different criteria:  
1. your current experience to solve a problem in, or using, ***X***,
2. and the time you are ready to spend for this.

We all know that time is not something that you can "buy“.
Therefore, logically, multiplying many "little" experiences on many things will force you to increase the time to be "just good" at one simple task, and can discourage you more easily to become an expert in one task.

As a personal opinion, I encourage people to become an expert on only one thing (based on a defined context, or subject), and just "good enough" in other subjects or technologies they are interested in.

## Features come with bugs

If you look at big projects like the Linux kernel, or the Chromium browser, you can notice that those are not bugs free.  
As you might know, the management of those big projects introduces a hierarchy to validate new features, or bug fixes.
The final step of this hierarchy for every merge request (new feature or bug fix(es)) is validation (and merge the feature of the fix to the original code base), or rejection, **after** pair reviewing.

So, when you discuss about a bug in production, you imply that both the developer and the reviewer have their own responsability here, as they missed the bug before the deployment.

Developers have to accept that **features come with bugs**, and that the most trickiest ones are, sometimes, invisible to humans.

Most of the time, **a developer is not a quality assurance**.
But, some solutions exist to help developers and reviewers in finding easy and tricky bugs in code: code analyser, monkey testing, chaos engineering, or just [tech switching that proposes security by design](https://medium.com/tadaweb/security-by-design-a-brief-introduction-to-rust-378060e45038) if you are developing systems or features in critical projects.

## Stop to compare with IT known figures

In order to know how we perform in one task, we often feel the need to compare our work with another person.  
When developers do this, they tend to compare what they are spending most of their time on: their code, or the software project(s) quality and popularity they achieved, or they are working on.  
This comparison can exists with colleagues, but also with known IT figures.

Those known figures are people who contributed actively to an important part of the IT history, and made a lot of efforts to build beautiful things we are still using today.  
For example, we can mention Ada Lovelace, Grace Hopper, Ken Thompson, Dennis Ritchie, Brian Kernighan, Margaret Heafield Hamilton, Steve Wozniak, Roberta Williams, Linus Torvalds, ... and many other persons, coming from different Computer Science subfields.  
Those persons are mainly considered as "rockstars" because **they are considered as "modern“ computing pioneers**.

Indeed, in order to solve real problems, those persons had enough imagination to propose **new** "standards" on low-level constraints: most of the time hardware configuration and the operating system used by users.

The book "Masters of Doom", from David Kushner, is a perfect example here. This book relates how two geniuses, John Carmack and John Romero, co-created (at least) two revolutionary games ("Doom" and "Quake"), which revolutionized computer graphics and game development.  
In "Masters of Doom", you realize quickly that computers at this time were still in its infancy... and that we cannot compare the work of those pioneers to the work of our engineers today.  
***Note***: *If you are more interested in game engines, I recommend you "Black Book: Wolfenstein 3D" and "Black Book: Doom" from Fabien Sanglard, who made incredible researchs to explain how they successfuly built from scratch those masterpieces.*

Today, software constraints are really different and, I would say, more complex.
Instead of building complex pieces of code on a unique hardware or software stack, most of the developers now are using different standards to ship every day **very** complex piece of codes, on **very** complex stacks, in **different private versatile environments**.

This implies today multiple technologies, and communities, to build one common thing using different tools.

To break down an open door, developers tend to be more "users“ today than "programmers" in the past.

## Conclusion

Even if developers try to save the world each day, they don't have superpowers (and they are not machines either).  

If you are a developer, do not spend your time in trying to become real expert everywhere, everytime, on everything, it's impossible.
Instead, you can try to be a real expert in only one thing more quickly, and just to be "good enough" in other fields or using other technologies.
