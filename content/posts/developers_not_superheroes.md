+++
title = "Developers are not superheroes"
date = 2020-04-24T14:00:00+02:00
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
The "super code monkey" sleeps well.  
The "super code monkey" is young, has a social life, go outside oftenly, and has the time to make physical exercise each day to keep in shape.

The "super code monkey" is an expert in Android and iOS apps development, he knows how to write excellent code in C, C++, and Java.  
The "super code monkey" studied Data Science in three weeks, and became an expert on Python, R, Matlab, Octave, and Julia in four weeks.  
The "super code monkey" has strong knowledge in Docker and Kubernetes, and used ten different Javascript frameworks the last six months.

And like every myth...  
The "super code monkey" does not exists!  
Except maybe in the “requirements file" of some IT recruitment companies...

Ok, let's destroy this myth.

## What is an "expert" ?

To me, **it's impossible to master a technology in a short period of time**.  
You can be "good" at it, but you **must** have a *strong* experience to declare in order to be considered as an expert.

Let's define what I understand when someone tells me he is an "expert", or that he "masters" something.

To me, you can't say that you are an expert in a given subject without speaking of the context you apply to it.
Also, you can't be an expert in something without maintaining a recurrent technological watch on the subject you are mastering.  
As an example, a data scientist can claims that he is an expert on Python for Data Science, and the usage DS modules like Pandas, or Scikit-Learn. But this is totally different for a network engineer, or for a software pentester.  
All three will be able to right very good Python code, but differently *because it is the context of the application that is important here*.

Becoming an expert on ***X*** depends strongly of two different criteria:  
1. your current experience to solve a problem for ***X***,
2. and the time you are ready to spend for this.

We all know that time is not something that you can buy and spend easily.
Therefore, logically, multiplying many "little" experiences on many things will force you to increase the time to be "just good" at one simple task, and can discourage you more easily.

Personally, I encourage people to become an expert on only one thing (based on a defined context), and just "good enough" in what they are interested in.

## Features come with bugs

If you look at big projects like the Linux kernel, or the Chromium browser, you can notice that they are not free of bugs.  
As you might know, the management of those big projects introduces a hierarchy to validate new features, or fixes.
The final step of this hierarchy for every merge request, for a new feature or for a bug fix, is validation (and merge the feature of the fix to the original code base), or rejection, **after** pair reviewing.

So, when you discuss about a bug in production, you imply the responsability is splitted in two: half for the developer that introduced the bug, and another half for the reviewer that did not caught the bug in time.

Developers have to accept that **features come with bugs**, and that the most trickiest ones are, sometimes, invisible to humans.

Most of the time, **a developer is not a quality assurance**.
But, some solutions exist to help developers and reviewers in finding most easy and tricky bugs: code analyser, monkey testing, chaos engineering, or just [tech switching that proposes security by design](https://medium.com/tadaweb/security-by-design-a-brief-introduction-to-rust-378060e45038) if you are developing systems or features in critical projects.

## Stop to compare with IT known figures

As human, we often need to compare our work with another person, in order to know how we perform.  
When developers do this, they tend to compare what they are spending most of their time on: their code, or the software projects they achieved.  
This can be with work colleagues, but also with known IT figures.

In IT culture and history, those known figures are people who contributed actively to an important part of the IT history, and made a lot of efforts to build beautiful things we are still using today.  
For example, we can mention Ada Lovelace, Grace Hopper, Ken Thompson, Dennis Ritchie, Brian Kernighan, Margaret Heafield Hamilton, Steve Wozniak, Roberta Williams, Linus Torvalds, John Carmack, Kevin Mitnick, ... and many other persons, in many Computer Science subfields.  
Those persons are mainly considered as "rockstars" because **they are considered as "modern“ computing pioneers**.

Indeed, in order to solve real problems, those persons had enough imagination to propose **new** "standards" on simple constraints (most of the time hardware configuration and operating system used by the user).

The book "Masters of Doom", from David Kushner, is a perfect example for this point, which describes how two geniuses, John Carmack and John Romero, co-created (at least) two revolutionary games (Doom and Quake), and how those games revolutionized computer graphics and games development today.  
In "Masters of Doom", you realize quickly that the computer age was still in its infancy... and that this age **can't** be compared to today.  
***Note***: *If you are more interested in game engines, I recommend you "Black Book: Wolfenstein 3D" and "Black Book: Doom" from Fabien Sanglard, who made incredible researchs to explain how they successfuly built from scratch those masterpieces.*

Today, software constraints are really different, and I would say more complex.
Instead of creating and proposing open standards that can live for many years, and building complex pieces of code on an hardware or software stack, most of the developers now are using those different standards to ship every day **very** complex piece of codes, on **very** complex stacks, in **different private versatile environments**.

To break down an open door, developers tend to be more "users“ than "programmers" anymore.

## Conclusion

Even if developers try to save the world each day, they don't have superpowers (and are not machines either).  

If you are a developer, do not spend your time in trying to become real expert everywhere, everytime, on everything.
Instead, you can try to be a real expert in something more quickly, and just to be "good enough" in what you are interested in.
