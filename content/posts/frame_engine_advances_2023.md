---
author: "Antonin"
title: "Update on Frame engine (2/x)"
date: "2023-08-13T15:00:00+02:00"
tags: ["programming", "swift", "swiftui", "apple metal", "game engine", "frame engine"]
description: "\"Making games is hard, y'know\""
toc: true
readTime: true
---

Almost one year ago [I posted my advances](/posts/frame_engine_advances_2022) on "`Frame`", a custom game engine wrote in Swift with SwiftUI, and Metal as a backend.

From August 2022 to August 2023, I made a **journey** into two different graphics libraries: _Vulkan_, and _Metal_.  

## The new project

I definitely wanted to take a deep look into Vulkan for multiple different reasons: i) it is cross-platform, and ii) Vulkan begins to be widely used as a "standard" for game engines rendering backend, iii) I needed to get deeper for my work.
Indeed, since July 2023, I port at RedArtGames a Vulkan game engine for all the modern consoles, and it made me curious to learn more, more, and more and this graphics library.

Metal is great and very (very very) powerful, or at least powerful enough for me to create good 2D and 3D experiences for mac, iPhone and iPad users.
**But** the issue with Metal is that it is used for Apple platforms only.
You can compile your shaders on Windows now, but you can't (still?) run Metal application from other platforms.  
So, I took my bag and got into a journey to create a tiny 3D rendering program portable using Vulkan and Metal as backends, which could run on Windows, macOS and iPadOS.
At the end, after a year, this "tiny 3D rendering program" became my new graphics engine...

There exist some Vulkan binding projects for Swift, like SwiftVulkan, but I prefered this time a simpler approach, developing this program in good-old C++ (17).

## The issue with Vulkan

First, I got into the documentation of Vulkan, which is very large and which you can simply fall into... and getting more confuse at the end.
It is a very strange feeling: the documentation is very dense, too much straightforward most of the time (no context of the error at all for example), and when you get answer then two other questions pop in your mind based on what the documentation describes as the error...  
Very strange.

The Vulkan API (`1.3.211.0` in my case) is **too** verbose, and there is a rumor thats says you will have to write 300 lines of code before rendering your "_hello-world GPU graphics programming program_" : the famous RGB triangle.  
**Yes, it is 100% true.**  
I don't have any issue with verbose APIs **but**, to me, verbose APIs have to be verbose for a reason: like very long names to avoid unnecessary comments or dev doc, to let the reviewer understands what the code is doing or what is the current process in the graphics pipeline, etc. 
To me this is not the case for Vulkan.  
What is the purpose of setting all your pipeline parts in your code if it has to be default one most of the time?  
What is the purpose of implementing a wild-variety of non-typed enums if it is not for making mistakes and scratch your head for 15 minutes?    
The API structures / classes are large and it is **very** easy to make a mistake, simply put a wrong enum, and lose 3ms in performance because of this...

A nice thing about Vulkan otherwise is the internal validator.
The warning / error messages are good enough to get what is the problem right away, including a hyperlink to the online documentation.

In terms of documentation, I think there is no official tutorial for Vulkan.
I found out a very old tutorial called "_Vulkan Tutorial_" but, after looking at the description and the code, I wouldn't recommend it to anyone as it is very outdated and contain multiple mistakes.

## Let's back to the engine...

My engine has two backends now :
* Vulkan, used on Windows and macOS, using VoltenVK,
* Metal, used on macOS, iPhone and iPadOS, and using [Metal-Cpp](https://developer.apple.com/metal/cpp/) layer for C++.

For the cross-platforms thing, I used the old-friend CMake to be assured that the engine could be built & run almost everywhere.
However one of my friends advice me to look at another build system called [`premake`](https://premake.github.io), which could simplify a lot all the CMakeFiles files I had to create to make everything portable. 

I implemented a debug view using [ImGui](https://github.com/ocornut/imgui) (again, big applauses for ocornut and the ImGui community for this awesome work), a nice graphical interface that runs almost everywhere, and the nice Docking features.

I implemented all the conditions to have a "modern" scene:
* a proper Scene architecture for thousands and thousands of objects to compute and render,
* a multitude of cameras in the Scene object, and a way to switch from one to another very easily,
* a proprietary solution to package 3D models, textures, ...
* multiple lighting solutions for the environment,
* a very simple collisions solution for objects in a scene,
* and a basic editor to quickly drag and drop objects from the computer to the virtual scene, move objects, move the scene's main camera hitting "PLAY", etc.

Also, I have implemented almost all of the things I had in my bucket lists from last year, except to let the developer modify and integrate its shaders via the game editor.

I still don't have a physics engine and sounds interpreter, as well as a game interpreter to run the objects scripts, but this is on list for August 2024... if I can find the time!

## All it takes

A game engine is a very complex piece of software, and as all other complex softwares everywhere else, it costs in developing features, in refactoring, in debugging, in profiling, ... It costs **your time**.
To find the time to search and make all of this, once the kids are in bed after work, twice a week at most, is a challenge for me today.

I am very proud of what I learned and what I did after this one-year journey.
Metal is definitely a simpler API compared to Vulkan, just comparing the API complexity together, and I continue to think that Vulkan should have taken the same path as Metal in terms of simplifying the API, while offering great customization of the graphics pipeline **if only necessary**.

**See you in August 2024 for the third update 👀**

## Screenshots

![A lighting scene, rendered on iPadOS](/images/frame_advances_scene_ipad.png#small)

A lighting scene, rendered on iPadOS

![A small part of the editor and its basic debug interface](/images/frame_advances_scene_editor.png#small)

A small part of the editor and its basic debug interface
