---
author: "Antonin"
title: "Know your threat model"
date: 2020-12-31T18:00:00+01:00
tags: ["security", "linux", "hardening"]
description: "Do not spread paranoia"
toc: false
readTime: true
---

Last week, a [very good blog post](https://madaidans-insecurities.github.io/guides/linux-hardening.html) has been published about how Linux **by default** is "not a secure operating system"... (kernel actually).  
I agree with most of the ideas and methods explained in it, **but** this article has a big issue: the target is absolutely not defined.
Unfortunately, this has been posted on hackernews (_oops_) this morning (_iich_) and, as expected (_ouuh_), it has created a lot of comments and criticisms (_oh my..._).

After a first read, you can think that Linux is absolutely **not** a good / decent kernel, which is absolutely false!
Linux is an **amazing** open-source kernel, built by thousands of amazing contributors all over the world, making a lot of efforts to make the kernel the most generalistic (it runs on various architectures), fast, and secure as possible.  
However, if you think that Linux is a secure kernel, you are **wrong**, especially because of the first rule of computer security: **absolutely no system is secure**.

We all know that the most common GNU/Linux users are **nerds**: people who love computers, test multiple (new) operating systems or softwares, and **hack** most of the time... or just people who think that Linux is "cool" and install it on their PC just "to feel like an'hacker, y'a know" (:facepalm:)...

When reading a blog post like this, which can be summarized as "a GNU/Linux distro is not secure _by default_", it resonates mostly for paranoid people and people who are scared about their "privacy"...  

***Note:*** _don't forget that "security" and "privacy" are not synonyms, and are not necessarily connected together. You can have a very secure OS where developers could fix all the bugs fast based on all your logs, which can be a privacy nightmare for you._

But first, I will explain the three different categories of persons who are using a GNU/Linux distribution today, and their specific threat model:

1. the students, the isolated nerds, the "random cool guys" in a small town somewhere or some random programmers all over the world, the /r/unixporn participants, etc. : you are **not** the target of this article.  
Sorry to tell, but _**nobody**_ is interested in knowing which kind of tools or vim themes you are using, what you ordered for your family recently at Christmas, or which porn's categories you are interested in Pornhub.
Even Google, Facebook, or the NSA does not really care about you (except if you really did something unusual...).  
You are just **a number**, an ID inside a database column.
Nothing more.
2. the company's members, especially employees: people who want a minimum of security to avoid targeted hacks, which are **not** making people lives in danger.  
Depending on what / where you are working, you **can** be a potential target, and you can be interested in this article.
As always, if you work in a company, be careful about what you are clicking on, but mainly on social engineering techniques: sometimes, some people just need to make a phone call to get all the informations they want...  
If you are in this specific category and don't know how to start, please ask some advices to your IT department, or to do that for you.
3. finally, the persons who are **deeply** concerned about high security and privacy levels, like the journalists, (h)activists, whistleblowers, computer scientists who work in security and / or on critical systems, big companies like banks, and security agencies...  
People whose lives (or other persons lives) may be in danger.  
For those, I think this blog article can be a good guide to make their life a bit less "paranoid", and reduce at the same time the surface of attack of their device(s).  
For those persons, I include that some good tools like Qubes or Tor could be a game changer for them, in their daily life.

![](/images/authorization.png#small "Comic from xkcd, 'Authorization' (https://xkcd.com/1200/)")

Whether you are part of the 1st, 2nd or 3rd category, even if you don't use a GNU/Linux distribution but another OS like Windows or macOS, the following (*basic*) rules can help you to manage your security:

* **encrypt your data** - if you are tweaking your kernel but did not thought about encrypting your data... oh boy...
* **set a firewall** - put it to basic (no ssh by default, accept all that goes out),
* **use a password manager** - some great tools like [Bitwarden](https://bitwarden.com/) are open source and can create strong passwords for your online accounts... but please to create a strong password to open your password wallet!
* **do not click on everything, everywhere**, and be careful about what you install on your kernel (just because _"RGB IS SO COOL ON MY KEYBOARD MAN!"_ does not means that you have to install a specific driver in your kernel space :/).
