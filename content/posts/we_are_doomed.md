+++
title = "We are doomed"
date = 2024-01-14T20:00:00+01:00
description = "A pessimistic view of \"modern software engineering\""
draft = false
categories = ["thoughts"]
tags = ["thoughts"]
disqus = false
+++

I wanted to write about modern graphics APIs at first, how exclusive they are depending of the platform you are 
developing on (and for), and how difficult to use they are.
However, after (over-)thinking about this blog post, I came to the conclusion that this problem could 
be generalized to all software.

This is a **very** pessimistic view of some people call "modern software engineering", observed by a 10 years professinal 
experience engineer and 20 years programmer.

Take it with one (or two) grain(s) of salt.

## Once upon a time, a developer and a graphics renderer...

Three years ago I began to work on my custom graphics renderer.
"Just for fun".

At that time I was working on macOS (an Apple Silicon hardware) so, naturally, I had four different graphics API available for my needs:
1. OpenGL: [deprecated in 2018](https://venturebeat.com/games/apple-defends-end-of-opengl-as-mac-game-developers-threaten-to-leave/) by Apple for their platforms,
2. Vulkan: not supported natively by Apple, but Khronos is maintaining Vulkan for macOS through 
[MoltenVK](https://github.com/KhronosGroup/MoltenVK),
3. Metal: the official **and exclusive** graphics API from Apple for their platforms,
4. My own software renderer: ... naah...

I felt a bit lost there.  
I would go for OpenGL **if** Apple did not deprecated the API (version 4.1, _circa_ 2010): 
simple to use, simple to understand, a great community if you have questions **and** a very good compatibility with 
existing hardware and other operating systems.  
Ok, so Metal? The main drawback of Metal is that it is **only** for Apple platforms, nothing else. I don't know what 
tomorrow will be made of, and I have absolutely no guarantee that Apple will not lockdown again their APIs / hardware / 
platforms and make my project completely useless in a few years... no go then!  
Ok, so Vulkan? As I said Khronos is maintaining a version of Vulkan for macOS using MoltenVK, a translation layer 
to Metal. I liked the idea **but** Metal is missing a lot of features compared to other hardware like AMD or Nvidia
hardware, and MoltenVK is like two years behind schedule compared to other platforms versions... hugh...

**At that moment I had the strange feeling that I was not choosing for the best graphics API, but the least worst of them.**

_**But how is the state of the graphics APIs on Windows by the way?**  
Fortunately OpenGL still exists (actually it is more a living-dead API), version **4.6**, 
depending of your hardware (but AMD / Nvidia did the updated for OpenGL 4.6 so your hardware should 
probably support it), and Windows has a great compatibility history to support old software and old DirectX versions.
But DirectX is **only** available on Win32 platforms like Metal is for Apple ones._

Ok, so we have four (4) different graphics APIs available, only two (_kinda-_)multiplatforms:
* OpenGL, version 4.1 on macOS and 4.6 on the others (_kinda_), but Khronos moved forward and is not interested on 
maintaining OpenGL anymore...
* and Vulkan.

Spoiler: I choosed Vulkan.  
The gap between OpenGL and Vulkan is abysmal, and I did realized that Vulkan was a sort a "bastard library": made 
for engine and graphics developers but its API is _so wild_ that it looks designed for device driver developers.
The documentation is unclear, the examples are quite complicated to understand and to adapt, and it seems that 
"extensions" principle can be used for very specific hardware versions, which is not what we want as a "product developer".

Developing features took time, and I had hard times developing them using Vulkan, but I continue to work on this project... until a few weeks ago.
I was sitting in front of my computer, on a performance issue **on Windows** until I realized the same code, on both 
Windows and macOS APIs, perform very differently. 
The Windows version was 30% slower than before, but the code performed better on macOS... I stopped there.

## Dedication to complexity

Looking back at when I first started programming when I was a kid (2002, on a old Compac PC running Debian) 
the things were pretty simple : just find a good documentation (at that time we shared some
very good technical books with friends instead of googling everything like today), the distribution was 
shipped with a C compiler and a text editor, the base standard library, and a very few APIs to make graphics.
If you want to exploit your dedicated hardware then you could use OpenGL (or other APIs) but most of the time you
exploited your entire CPU to render something on screen... **and it was very simple**!  
Also, we were running computers with 256MB of RAM, a single core CPU (less than 2GHz if I remember correctly) and 
software at this time were running blazingly fast! Tools, games, ... everything was running ok.

I have the feeling that software became (and still continue) very complicated and **very** expensive in terms of resources 
because of poor libraries and poor APIs, bloated operating systems, used by strong hardware but running poorly due to 
all of this.
Engineers tend to over-think an API through the piece of hardware that has to be exploited by the end developer.  
This is very true for GPU hardware manufacturer, where one dedicated piece of hardware has a non-standardized API to 
exploit it, which is not compatible with another manufacturer, and this is true also for console manufacturers like Sony,
Nintendo or Microsoft.
But this is true also for system software providers like Microsoft or Apple, through their own operating system and 
tools.

Complexity impacts everything now, from the programming language to the operating system, passing through the API you
are using to exploit your dedicated hardware, the "stack" you are developing your piece of code on, or the compiler
you are using.
We have very fast computers today which don't seem fast at all as soon as you try to compile a simple project with 
more than 20 C++ code files in it, mainly because the CPU is busy to run extra code and software running on your computer?  
A simple example: It takes 13 seconds to my new Windows computer to compile my 20+ C++ files project, or 780 frames
if you are developing a game at 60Hz refresh rate.  
Maybe because of Microsoft Defender, keeping an eye on newly created file in case of malware? 
Another process that analyzes the content of your computer every second, or another process that limits your hardware
 depending of your environment...? Who knows! 🤷

I wonder how much time the C++ compiler I had in 2002 would takes to compile a project with 20+ C++ files... 
13 seconds too maybe? :)

## Bloat is everywhere... and nobody seems concerned

The goal of a software engineer (or developer) is to solve real problems using a computer. 
I am very pissed when I see software "engineers" more concerned about a vim configuration than thinking
why their shell profile takes three to four full seconds to load.

As end users we have no choice to use what developers deliver.
But, as software engineers, we have no choice either to use what software and hardware providers deliver too, and the 
quality of the tools or libraries is becoming weaker and weaker year by year, version by version...

Developers today are definitely not productive than before, mainly because they have to deal with a mountain of 
libraries, APIs and abstraction, which increase the level of complexity of developing and releasing a **good** software 
for the end users, and distract them.

But do we have a choice here?  
Imagine that you are (like me) porting games and game engines to consoles : you will have to deal, again, with new tools,
new APIs, new abstractions, to build games **exclusively** using... Windows.  
No tools to develop GNU/Linux (there was a time 
[SCE released tools for Linux to develop on PlayStation 2](https://ps2linux.no-ip.info/playstation2-linux.com/coding-on-playstation2.html) 
but this time is now  revoked), no tools for macOS, ... just Windows.
So, you are **forced** (not encouraged, but forced) to use this operating system and tools (like Visual Studio), and to deal with its complexity, 
to release a product on another platform.

I don't blame the software developers here.
They have to live, and they mostly operate by people only interested in retention rates and how much end users spent on
their tools and platforms.  
Also, mostly do not care about performance and optimization anymore as their own computers have enough of RAM to handle 
the app itself, and do not even know how their own software is being executed... This is very sad to me.

As an example, I am still amazed the difference of time PhotoShop is taken now to open a simple image, compared to 10 years ago 
for the same picture: much less time, much less RAM, and much (_much much much much_) less HTTP calls too.

{{< youtube ZSRHeXYDLko >}}

## The "container" joker card

When I discuss about OSs behaviours I hear more and more oftenly this very unique sentence 
> OS does not matter anymore as we use containers now...

So, based on the _fact_ I could run, deploy and "save" my software in a container for 10 years, does it means I could develop 
and run a game engine on a container and deploy the image everywhere **without** platform incompatibility?  
If yes... did you hear about the Docker incompatibility when Apple Silicon chips have been released...? :)
Or maybe another platform incompatibility?

As Docker mentions in their documentation:
> Every Engine release strives to be backward compatible with its predecessors, and interface stability is always a priority at Docker.

followed by
> In all cases, feature removal is communicated three releases in advance and documented as part of the deprecated features page.

This implies that, if you find an old image you want to use, you must definitely use the version of the engine that
was used originally to develop and build the image **if** your system still supports it.

The **only** system with a good software compatibility that I know is Windows, and this explains a ton of things keeping
very old UI/UX frameworks, software and APIs to run, for example, Windows 95 compatible games like 
"Roller Coaster Tycoon".

Otherwise, **you are doomed**.

![we_are_doomed_conclusion.jpg](/images/we_are_doomed_conclusion.jpg)
Generated using [memegenerator](https://memegenerator.net).
