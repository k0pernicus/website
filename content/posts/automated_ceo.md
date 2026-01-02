---
author: "Antonin"
title: "The CEO cost function"
date: 2026-01-02T13:30:00+02:00
draft: false
description: "Looking for an Algorithmic Executive"
tags: ["thoughts", "ai"]
toc: false
readTime: true
math: true
---

We are living through a strange era in tech.

Every month, a new headline appears. Another CEO announces a massive round of layoffs of managers, developers, and engineers.  
They talk about "efficiency" and "streamlining", which should maintain the same quality while spending less on staff.
However the engineers, and the users, on the ground see something else: [a decrease in software quality](https://www.windowscentral.com/microsoft/windows-11/2025-has-been-an-awful-year-for-windows-11-with-infuriating-bugs-and-constant-unwanted-features), [a loss of institutional knowledge](https://www.windowscentral.com/microsoft/windows-11/microsoft-ai-ceo-pushes-back-against-critics-after-recent-windows-ai-backlash-the-fact-that-people-are-unimpressed-is-mindblowing-to-me), and [a product that starts to feel like it’s held together by duct tape](https://www.reddit.com/r/softwaredevelopment/comments/1oy40of/ai_coding_tools_ruining_code_quality/).

Meanwhile, [these same CEOs see their incomes rise](https://fortune.com/2025/10/22/microsoft-ceo-satya-nadella-pay-hits-a-record-96-5-million-dollars-usd-he-got-a-22-percent-pay-rise-mirroring-the-4-trillion-tech-giants-skyrocketing-shares/).

It makes me wonder. If a CEO's job is essentially a series of inputs and outputs—decisions in, stock price out—can we finally treat that role as a cost function?  
If we can model a complex distributed system, why can't we model the person at the top?

## The formula of a leader

History is surprisingly clear on this. We have decades of data on stock movements, economic strategies, and the specific decisions that led to them. We have thousands of articles explaining exactly why a pivot worked or why a merger failed.

When you strip away the charisma and the expensive suits, **a CEO is just an optimization problem** over an original idea.  
They are trying to maximize a reward function ($J$), which can be simplified as:

$$J = \sum_{t=0}^{T} \gamma^t (w_1 \cdot R_t + w_2 \cdot S_t + w_3 \cdot U_t - w_4 \cdot Q_t)$$

* $R_t$: quarterly revenue,
* $S_t$: stock price / market cap,
* $Q_t$: system quality (technical debt, employee retention, bug count, ...),
* $U_t$: user satisfaction (measured via churn rate, NPS, sentiment analysis, ...),
* $w_n$: weights assigned by the board of directors (likely for priorities).

The problem with human CEOs is that they often set $w_3$ (user satisfaction) or $w_4$ (system quality) to zero if the sum of $w_1$ and $w_2$ (revenues and stocks) are higher. 
Their incentives (mostly vesting schedules and annual bonuses) encourage them to maximize $R_t$ and $S_t$ at all costs.  
A good AI, however, could be hard-coded with a high $w_3$ value.  
Then, it would be mathematically incapable of "winning" if software quality drops below a certain threshold.

Human CEOs are expensive, they are prone to ego-driven mistakes, and [they often "bullshit" to cover their tracks](https://abc7news.com/post/artificial-intelligence-ceo-san-francisco-tech-company-replit-apologizes-ai-chatbot-goes-rogue/17295335/). 
An AI wouldn't buy a social media platform on a whim just to prove a point. It would be transparent. It would explain exactly why it chose to cut a budget or invest in a new feature.

Most importantly, an AI would cost a fraction of the [1,200% increase in CEO pay we've seen since 1978](https://www.epi.org/publication/ceo-pay-in-2022/).

## Surgical cut

In the current corporate playbook, layoffs are a blunt instrument. When a CEO needs to save money, they often fire the most expensive people: the Senior Engineers.
This has been explained, once again, by [AWS CEO a few months ago](https://www.finalroundai.com/blog/aws-ceo-ai-cannot-replace-junior-developers):

> “Number two, they [**the junior developers**] are usually the least expensive because they're right out of college, and they generally make less. So if you're thinking about cost optimization, they're not the only people you would want to optimize around.” AWS CEO Matt Garman

But the senior people are the same people who know where the "bodies are buried" in the codebase. 
Removing them to save the budget is like a pilot throwing the engines overboard to make the plane lighter.

An AI CEO would see the company not as a list of salaries, but as a **dependency graph**.

If the platform’s stability is failing, the data is usually screaming the reason why. 
It might show that the "code-to-bug" ratio is spiking in a specific department, or that "time-to-resolve" is dragging because of a bottleneck in management.

In this scenario, the AI wouldn't fire the engineers. 
It would look at the data and realize that the engineers are actually performing well, but are being slowed down by three layers of middle management that produce nothing but meetings. 
The AI would perform a surgical cut: **it would remove the bureaucracy to save the builders**.

## Objectivity and Truth

By using an AI to manage the "human resources" of a company, we remove the politics of survival. 
In a human-led company, the person who survives a layoff is often the one who is best at "managing up" - the one the CEO likes personally. 

An AI doesn't have "favorites". It only has metrics.

This brings us to a new kind of **corporate transparency**. If you are fired by an AI, you get a receipt. You see the data. You see the logic. If your contribution to the system's health is high, the math protects you. If your role is purely administrative and the data shows you are a net-negative on velocity, the math removes you. It’s cold, yes, but it’s honest.

## The alignment problem

There is a danger here, of course. If we train an AI on historical data, it might learn the wrong lessons.

As an example, if the history shows that every time a company fires 10% of its staff stock price jumps for two months, then the AI will keep firing everyone until there is no company left. 
This is totally normal for the AI because it tries to maximize the reward at the expense of survival. 

This is the classic "**alignment problem**".

There is also the risk of "**corporate monoculture**".  
If every company uses the same CEO model trained on the same data, every company will act exactly the same. 
They will all chase the same trends and be exploited by anyone who figures out the algorithm.

And then there is the **morality**. 
An algorithm doesn't care about the families affected by a decision.
It only cares about the math.

## Self-correctness

The solution of alignment problem and corporate monoculture isn't just a better model, but **better engineering**.
We need **exploration**, not just exploitation of past data.

We can imagine a system where the AI is not just a decision-maker, but also its own judge. 
As an example, we could have a "self-introspection layer", a secondary model that audits the decisions made in the previous quarter and refines the decisions for the next round.

If the "CEO Model" predicted that firing the entire QA team would save $22M without affecting quality, but the "auditor model" sees that bug reports increased by 40% and user retention dropped, the system corrects itself. It adjusts its own "cost function". It learns that short-term savings are actually long-term liabilities, **for the company it tries to manage**.

This is something human CEOs rarely do as admitting a mistake is politically expensive for a human CEO. However, for an AI it is just a weight adjustment of its function.

## "Augmented CEO"

We aren't ready to hand the keys of the boardroom to a server rack just yet. However the "augmented CEO" is already becoming a necessity.

Imagine a board meeting where every proposal is stress-tested against 10,000 simulations. The CEO says, "We need to layoff 500 engineers to hit our quarterly targets".
The AI responds, "That decision has an 85% probability of causing a system-wide outage within 12 months due to lost expertise".

In this world, **decisions become data-driven instead of gut-driven**.
It forces a level of honesty that the current corporate structure lacks, or do not want to listen on purpose.

Also, to have a companion for that would reduce the bonus and salary as the CEO is not anymore the executive, 
but just an "interface" between the AI and the board of directors.

The conclusion is simple, even if it's uncomfortable for those at the top. The idea that certain roles are "too human" or "too complex" to be automated is a myth.  
History shows that no one is truly irreplaceable.
Once the cost function is solved, not even the CEO is safe.
