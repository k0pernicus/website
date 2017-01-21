+++
description = "k0pernicus 2.0"
title = "My new GNU/Linux configuration"
date = "2017-01-21T14:51:03+01:00"
tags = ["GNU", "linux", "configuration"]
author = ""

+++

Yeah, I get back from macOS to GNU/Linux, and I am really happy !

During the last French sales, I bought a new laptop to replace my 2013 Macbook Air: a Zenbook from Asus.  
This Zenbook is the UX310, with 8 Gbytes of RAM, 256 Gbytes of SSD, an i7 processor and, finaly, a QHD+ display screen (which is **awesome**).
The pros of this laptop are that it runs approximately 10 hours (on a GNU/Linux distribution like Ubuntu), a great keyboard, an awesome screen and a great compatibility with the major GNU/Linux distributions.
The cons are a mediocre touchpad and a lack of USB ports - this laptop offers a USB 3.0 and USB Type C connexions, but I had difficulties to plug the USB key that contains a GNU/Linux distribution, and a USB/Ethernet device...

First of all, during the shipment process, I was looking for 2016's GNU/Linux distributions that can be easily installed on this laptop.  
I noticed four distributions: [Fedora](https://getfedora.org/fr/), [ArchLinux](https://archlinux.fr/), [AntergOS](https://antergos.com/) and [CoreOS](https://coreos.com/).  
The both first distributions are known in the GNU/Linux universe - I think that they are in the Top10 GNU/Linux distributions on [distrowatch](https://distrowatch.com/).
Otherwise, the last two are: an helper distribution to install easily an ArchLinux distribution, and a lightweight OS designed for providing infrastructure to clustered deployments.

I am interesting about running containers on my desktop since I read some [blog posts from Jessie Frazelle](https://blog.jessfraz.com/), a software engineer at Google, and a maintainer of Docker.  
Unfortunately, I do not feel really comfortable with the idea to use an OS that runs **only** Docker containers...  
So, I tested AntergOS and... It works **really** great :-)

I think actually that GNOME is a good desktop solution - I had some problems using KDE a few years, and it seems that Plasma is not stable yet.  
However, I plan to migrate from GNOME to XMonad soon - maybe a few weeks.

Today, I use those softwares every day on my laptop:

* _irssi_, an IRC software that runs on console;
* _neovim_, the VIM of the XXIth century;
* _firefox_, ... seriously;
* _thunderbird_, ... seriously (again);
* _transmission_, a famous torrent client;
* _vlc_, the French awesome music and video player.

Finaly, I installed also some environments to program with _Rust_ (my favorite programming language), _OCaml_ (the greatest functional programming language for me) and _Nim_ (a language that deserves attention for meta-programming, at least).

The combination AntergOS/GNOME works great - apart that screen backlight keys do not work, everything is working great on my laptop :-)
