+++
title = "Slow... for your security!"
date = 2020-11-13T17:30:00+02:00
draft = false
categories = ["macOS", "apple"]
tags = ["macOS", "apple"]
description = "macOS Big Fail"
disqus = false
+++

**EDIT (15th of November 2020)**  
_I made a mistake in explaining that OCSP send in its request the hash binary, which is not correct, and I am sorry for that._  
_Indeed, OCSP send out the hash of the app developer certificate, as explained [in this great blog post](https://blog.jacopo.io/en/post/apple-ocsp/)._

Apple released it's new mac operating system update - macOS Big Sur - yesterday night, a version that introduces a new design, closer to the iOS / iPadOS interface, new security updates (goodbye hacky kernel extensions...), new features & apps via Catalyst and... an optimized version of macOS for Apple Silicon devices.

## First, the hype...

The update night was a big "update party" for many people, as Big Sur is a trully big next step for the mac, and for its users.

It **was** a "big update party", but not for the good reasons...  
Indeed, in one single night, Apple demonstrated to everyone the main drawbacks of their "new security updates and protocols for macOS", and it was bad, really bad.

## ... then, the outages

At 7PM yesterday, Apple released Big Sur.  
People got the notification, shared the excitement, and ~~downloaded it~~ tried to download it.

The image size of Big Sur to download was **12.1 GB**, which is **4 GB more than the previous version of macOS**: Catalina.

The image is huge, and I do think that, despite the latest precautions taken by Apple, the servers were not fully ready to handle the hype... which leads to an outage, a worldwide fail, and a lot of good memes.

{{< tweet 1326999537790185473 >}}

Then, the situation gotten worse and worse as Apple decided to release, during the outage, new beta versions of many tools and operating systems (iOS, watchOS, ...) for the developers.

In parallel to this situation - and I don't know exactly if the following issues were related to the Big Sur update - the Apple Developers app kept crashing all the time, Apple Card and Apple Pay were under outage also, iMessage stopped as well as Apple Plans APIs and big services for the developers... and the final customers.  
And this, as a domino game, leads to another fail...

{{< figure src="/images/macos_big_fail_system_status_apps.png" title="Apple System Status during the outage, for apps and APIs [1]" >}}

{{< figure src="/images/macos_big_fail_system_status.png" title="Apple System Status during the outage, for devs [1]" >}}

## Slow by design

I stopped very quickly to try to download Big Sur, as many people all over the world were complaining about the situation, and decided to work on my own stuff... but then, I noticed that opening apps like Firefox took me **almost 20 seconds** on my latest Macbook Pro, as well as iTerm, VSCode, and many other dev apps (because, yes, I am writing code in my spare time - blame me!).  
I restarted the machine, and got the same issues.

This was weird, but it became even weirder when I saw on Twitter that other developers have exactly the same issues, in the same time...

{{< tweet 1326991320519335936 >}}

Then, suddenly, Jeff Johnson proposed this solution:

{{< tweet 1326990296412991489 >}}

A lot of people reacted to this tweet as
> "Wait... how a network request - that I did not send - can slow down all my tools on my local machine?!".

Let me explain the magic behind `Gatekeeper`...

## #ForYourSecurity

Apple introduced in 2012 `Gatekeeper`, a "security-first tool" to make sure that all the softwares you launch, on your mac, have been **verified** by Apple itself, and do not contain any code intentionally designed to cause damage on user's computer.  
`Gatekeeper` has been recently updated (on macOS Sierra) to disallow the system to install software that have not been approved by Apple (downloaded from the App Store) *and* unapproved by the user manually.
Because of this, each user has now to allow manually, in the system preferences, that he wants to run *this* specific software on its own machine.

But, to check if a given software has been validated or not by Apple, `Gatekeeper` has to check the validity of the soft (~~a simple hash of the binary~~ a hash about the app developer certificate[2]) **over the internet to Apple servers** after a given time you opened it at first (**not each time** [2]).  
The service daemon is called `trustd`.

This means that if Apple servers are slow to answer, `Gatekeeper` will block the opening of the software until it got a response... which can take a while.

***Note**: yes, libc can make a phonecall!*

Introduce security checks like this to the detriment of the consumer is **ugly**, and the main decision that has been taken by a lot of developers yesterday, to run their software faster at that moment, was just to block the request from `trustd` using some great firewall tools, like [`LuLu`](https://objective-see.com/products/lulu.html) or [`Little Snitch`](https://www.obdev.at/products/littlesnitch/index.html).

Unfortunately, this method will not work anymore on macOS Big Sur as `trustd` can't be block by users anymore.

{{< tweet 1327034191523975168 >}}

## "Here comes the sun..."

Apple fixed **all** of those issues in less than two hours, hopefully!  
I will not blame the company for the outages, the update fail, all of this... but for the decision to maintain the disastrous architecture of `Gatekeeper` to the detriment of the mac user.  
This needs to be change, but the update pointed by Patrick Wardle does not presume anything good on this side unfortunately...

[1] Screenshots to credit to [9to5mac](https://9to5mac.com) - got from this [9to5mac article](https://9to5mac.com/2020/11/12/apple-widespread-outages-big-sur-downloads-catalina-imessage/)

[2] [https://blog.jacopo.io/en/post/apple-ocsp/](https://blog.jacopo.io/en/post/apple-ocsp/)
