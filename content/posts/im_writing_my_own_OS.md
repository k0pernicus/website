+++
author = "Antonin"
date = "2016-07-26T00:01:00-04:00"
description = "Kernel panic!"
tags = ["rust", "os", "diy", "intermezzos"]
title = "I am writing my own OS"
disqus = false
+++

My fascination with computers turned faster as an obsession.

I'm obsessed with my machine.
Really, I'm not feeling all right if I don't clean up my machine physically
every week, and if I don't reinstall a new clean operating system each 6 months.
Obviously, before each _clean install_, I overwrite my entire hard drive with
zeros... which takes me a full day for a 1 TB hard drive.

When I was using a GNU/Linux OS, and before installing a new operating system on my computer, I always look up on
[distrowatch](https://distrowatch.com) what's new in the GNU/Linux world.
What is the new fashion OS, new version of my favourite OSs, new forks,
new OSs, etc...
So, it happened (a lot) that I install a new operating system each 6 months, just
to test it, see what's new, the stability... and reinstall an ArchLinux maybe 3 days
after.

Archlinux, Debian and ElementaryOS are my favourite OSs.
Archlinux because it's really modulable, and you can install what you want without have
some garbage applications like basic mail clients, video and music players, or even
Amazon packets (just 4 you Ubuntu...).
Debian is really stable, but I install it only if my computer have more than 2 years, just to
be sure that packets to manage for example my GPU is accessible and can be install/run without
make some _wizard things_.
Finally, I use ElementaryOS a lot for recent machines, especially because the base of
this OS is Ubuntu, and I didn't have some troubles using Ubuntu packets for recent hardware.
ElementaryOS is pretty stable and Pantheon, the desktop, is really ergonomical and beautiful -
inspired by macOS.

The operating system is the second software that the machine launch, after
the bios.
The operating system manage, by itself, some sort of things: processes, interruption handlers,
memory management, events from external devices like Keyboard, etc...
It's a magnificient piece of engineering, between you and the hardware.

BUT, I don't know how works really an operating system.
I mean, I take some courses like "Architecture of Operating Systems" at university, I've got a large
comprehension of what an OS do and how he can do that, but I never programed one.
And, in computer science, like some teachers and research guys told me: *"If you want to learn or just
want to know if you had understand something, try to program it!"*

I'm using [Rust](https://www.rust-lang.org) since 1 year now, and it's a wonderful programming language.
This language has been developed by the Mozilla foundation to avoid some problems
due to using C++ for their products, especially memory leaks and error management.
In june, a first version of [Servo](https://servo.org), the faster and most memery efficient modern browser engine ever made,
has been released in a basic web browser - just a public ALPHA. And the project is REALLY AWE-SOME!
Rust rised up recently the 1.10 stable version, and a lot of awesome projects are currently been build
using this language.

Personaly, at the beginning, I used Rust a lot to port my slow Python projects into a faster and memory efficient program, but also
during my combinatorial optimization courses, to prone usage of modern programming languages and also "4 fun".
During October/November 2015, I started to contribute to [RedoxOS](http://www.redox-os.org), a hobby operating system.
RedoxOS works actually on Qemu and VirtualBox, and is written in Rust.
The perfect project for me to contribute to a cool project, learn some things about operating systems and to
improve myself in a cool programming language like Rust.

But the project is really big, there is a lot of contributions per week and the code is optimized to be not understandable sometimes...

So, to perform myself in 1) contributing to build the best operating system in Rust as possible and
2) improving myself in Rust, I decided to create my own operating system in Rust.

I called it **arOS**, for _another rust Operating System_.

[Steve Klabnik](http://www.steveklabnik.com), a famous Rubyist, just started a project called [intermezzOS](http://www.steveklabnik.com).
This project is an hobby operating system, written in Rust.
intermezzOS is open source, and anyone can see the code and reflexions behind this project,
using the [online book](http://intermezzos.github.io/book/).
Steve made this project by himself, "just 4 fun".
Also, as the project is currently building, he release some news and code regularly.

Currently, the final chapter is about how to link Rust code in your boot assembly code, which is the base
to begin to program your own OS in Rust.

During 4 days, I followed the tutorial of Steve Klabnik.
[My code](https://github.com/k0pernicus/arOS) is kinda similar to his one, but I'm completing
my learning programming a VGA driver by myself, which is the next chapter of the tutorial (and it's not easy, trust me...).

During these 4 days, I was fighting with the _nightly_ version of Rust and his _feature_ tags.
These tags are incredibly usefull, but there is much features for the nightly version that
I'm completely lost sometimes...
It was the first time that I creating some _extern_ and _unsafe_ code in Rust, that is really
strange and completely exciting!
Also, I wrote some assembly code during my classes, but not as well as the boot and multiboot assembly code showed in the tutorial.

This project is really interesting, and I just learn usefull things about assembly code, how to link Rust code, and some
optimizations made by _rustc_, the Rust compiler, for projects which has to use less memory as possible.

This project is not really serious, just for learn and just for fun :-)

My next step, after the VGA driver, is to port this OS for the ARM family (a Raspberry Pi for example), using Qemu to test all long
the operating system.
