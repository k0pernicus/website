+++
title = "The 'intelligence' problem in 'artificial intelligence'"
date = 2018-09-22T13:38:34+02:00
description = "Too dumb for you."
draft = false
toc = false
categories = ["thoughts", "artificial intelligence", "machine learning"]
tags = ["machine learning", "artificial intelligence", "thoughts"]
disqus = false
+++

{{< figure src="/images/intelligence_problem.png" title="XKCD - Artificial Intelligence" >}}

The more I listen AI enthousiastic people, the more I can differenciate them in '10' very different groups:
* people who think that AI is a synonym of "strong black magic";
* people who are amazed by AI because "come on dude... it's just maths!".

There is something wrong with "Artificial Intelligence"...
I don't speak about some tech, just the term.

Let me explain why, in three different points.

## The human feeling

My teachers always told me:
> People think a task is not valuable as 'intelligent' as soon a machine can do it.

But... are we training machines to do something intelligent?

A few years ago, top HackerNews articles were about separating muffins and chiwawa,
playing go as an expert, or detecting earlier agressive cancers.
Are people intelligent because we can differenciate muffins and chiwawa?

Personally, I don't think so...
If you take the cancer example, we are actually transform a computer into a cancerologist expert, *to help him*.
And if you take the go example, we are actually transform a computer into a go expert player.

The same key between those two examples is "turning a machine into an expert of [put something here]", and that is what
AI is doing: to automatize tasks using an expert system.
Also, if we discuss about Machine Learning, we are using concrete examples to let the computer infer the pattern in order
to be an expert system.
Those data are processed by experts first, in order to clean them, and improve the score of the ML algorithm.

There is nothing "intelligent thing" here.
We are trying to **automatize** _something_ to save time, and/or save resources.

## The AI paradox

Two years ago, I discussed with my thesis supervisor about optimizing a task using Machine Learning, and I explained
that, with good features and intuition, I thought a logistic regression on the features could make real good predictions.
But, suddenly, he laughed really hard because he didn't understand why a logistic regression is classified as a machine learning algorithm as it's Texas Instrument
calculator could do the same thing.

The question behind this story is: "Can we restrict AI to algorithms that run only on strong hardware?"

Short response: **of course not**.
AI is based on simple algorithms, and simple paradigms, that can run on powerless hardware.
I agree that, today, a lot of complex algorithms are used to solve simple and complex
tasks: bagging and boosting algorithms, or neural networks for example.

But to minimize AI as "complex algorithms" is totally wrong.
**The algorithm used to solve a task is not the most important task in Data Science, it's feature engineering!**

## The "automatic"/"automatize" confusion

Many people think that AI is a magic process to learn something to a computer.
For those people, AI is a big black box which takes as input data, and output a value.
If we follow this idea, I really don't know why we need Data Scientists today...

Obviously, this statement is not correct - you can just train "something" on data without any interessment, but you will fail in the training process (otherwise, you are *very* lucky).

Actually, people are really confused between "automatic" and "automatize".
Computers **don't** automatically learn something - **we train them** using data for a given task.

The *goal* of Machine Learning is to *automatize* a process using data.
And there is no magic in the training, just mathematics...

## Algorithms for Automatization ?

For me, "intelligence" and "automatic" are two strong words to avoid we talk about AI.
As the main goal of AI is to automatize some taks using algorithms... why not "Algorithms for Automatization"?

