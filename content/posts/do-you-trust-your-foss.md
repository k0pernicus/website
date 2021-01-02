+++
title = "Do you trust your FOSS?"
date = 2021-01-02T16:07:00+02:00
draft = false
categories = ["open source", "thoughts"]
tags = ["open source", "thoughts"]
description = "The chain of trust problem"
disqus = false
+++

People tend to believe that free and open source softwares (_FOSS_) are **way more trustable** than proprietary softwares... and **I want to believe this idea**.

Unfortunately, the idea behind this blog post is that it is not because a software has a free or open source licence that someone already checked and validated its **full** source code (including the dependencies)!  
As an example, how many times did you put a star on a github project without checking the code at 100%?
Doing this, you can unfortunately promote a vulnerable library, which could be shared to your github followers who can believe that, as you trusted this library enough to star it is **obviously trustable**...

This is a typical **[chain of trust](https://en.wikipedia.org/wiki/Chain_of_trust)** problem.

## The first level of trust

To enable the confidence to a software, you have to:
1. trust the developer,
2. check and validate the source code of the project,
3. check and validate the dependencies of the project, and the dependencies of the dependencies, and so on...

This is already a **big** measure to be sure that the software you are installing on your computer can be trusted... as a minimum level!  
Especially if you want to install a big FOSS project like emacs... or even bigger: chromium or the linux kernel!

_**Note:** If you don't belive me about trusting your dependencies, please check this [NodeJS module CVE](https://nvd.nist.gov/vuln/detail/CVE-2020-7699) from 2020, which has been used seven million times._  

Let's imagine here that you checked everything, and that everything is fine...
The next step is to install the software on your machine.

## The second level of trust

The simplest solution to install a sofware on your system is... to use a package manager.  
Using this, you will have to:
1. trust that the package manager is installing the right package in your system (comparing the hash for example... if the hash function does not produce hash collisions!),
2. check and validate that the software, available in your package manager server(s), corresponds to the build of the source code you checked right before,
3. check and validate that the maintainer of this packet is also the developer and, if not, trust or not the maintainer.

Outch... this is becoming a more complex issue as you have to trust a lot of newcomers now...  
So, let's do like all Gentoo users: compile the software, wait for a few minutes / hours / days (if you want to install Chromium for example...) and install it yourself!

But the issue remains the same as the question now is: "**can you fully trust your compiler?**" ;-)

{{< youtube id="nt0ASzS4O6A" autoplay="false" title="A great presentation on how to introduce flaws into binaries via compiler extension(s)" >}}

## Conclusion

People tend to think that a software is trustable (and secure) because its licence is GPL or MIT, which tends to be wrong, especially since a basic CLI can be a weird gigantic pile of dependencies...

Don't get me wrong: I don't say that free and open source softwares are bad, and I don't believe to this idea!
On the contrary, I do think at 200% that FOSS are **absolutely** a necessity for developers and end-users.  
I am grateful for FOSS and all people that work on their spare time to develop great tools, free to use, free to modify, and free to distribute.
Unfortunately, some persons are taking advantage of this wrong "trust situation" to introduce widely new flaws and vulnerabilities...

The battle of trusting your softwares is unfortunately not yet won.
