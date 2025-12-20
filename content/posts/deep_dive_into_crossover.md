---
author: "Antonin"
title: "Deep dive into crossover"
date: 2025-12-20T15:00:00+02:00
draft: false
description: "A three generation hack"
tags: ["crossover", "macos", "gaming", "apple silicon", "wine"]
toc: true
readTime: true
---

If you read my previous post about [gaming on mac](/posts/crossover_mac_retro_games), you know I have a soft spot for running Windows games on Apple Silicon.

Early this year I upgraded from a M1 MacBook Air to an M3 Max MacBook Pro. 
Naturally, I had to test three completely different generations of games at it: [*ARC Raiders*](https://en.wikipedia.org/wiki/ARC_Raiders) (2025), [*Routine*](https://en.wikipedia.org/wiki/Routine_(video_game)) (2025), [*Watch Dogs 2*](https://en.wikipedia.org/wiki/Watch_Dogs_2) (2016) and [*Tron 2.0*](https://en.wikipedia.org/wiki/Tron_2.0) (2003).

I expected the modern game to struggle and the old game to fly. 
The reality was exactly the opposite, and it forced me to dig deep into how [CrossOver](https://www.codeweavers.com/crossover) handles translation layers under the hood.

This article summarizes my investigations and challenges to play DirectX 12, Direct 11, and DirectX 9 games, using Crossover.

## The Modern Stack: D3DMetal and MSync

### There is no place for anti-cheats

Let's start with the bad news: no, you still can't play everything. 
Despite the M3 Max being a hardware beast, there is a hard software wall called Kernel-level Anti-Cheat.  
I am not going to get into the debate of if it is useful or not, but if you are interested in that debate you can feed [this Steam forum discussion](https://steamcommunity.com/app/2807960/discussions/0/664963107066558385/).  
Games like *ARC Raiders* are strictly unplayable because Wine cannot emulate the deep Windows kernel access that Easy Anti-Cheat (EAC) or BattlEye require. If your target game relies on invasive anti-cheat then the Crossover solution is not the good one, as there is no "userspace bridge" built by Apple and / or Crossover to support those software.  
For that kind of games you might be interested in installing a Windows VM and then install Steam and your game(s) on that system, but this is not the purpose of this blog post. 

However, for the vast majority of single-player titles that don't treat the OS like a crime scene, the landscape has changed dramatically.

*ARC Raiders* : **Fail**.

### The routine

First, I tried the most recent game, *Routine*. *Routine* is made using Unreal Engine and uses the DirectX 12 graphics API under the hood.  
I tried this game on a default "Steam" bottle on Crossover, with "Auto" settings, and... it launched successfully! No graphics or performance issue, no issue recovering my saves through Steam cloud, and I successfully continued the game I began on my other machine.

Great!

*Routine* : **Great success**.

### The hackerspace

Next, I tried *Watch Dogs 2*. This game is almost 10 years old, and runs on DirectX 11.  
As *Routine* I tried with the auto settings, but the game did run poorly, and glitchees appeared too regularly.  
It was time to tweak the machine!

On modern versions of CrossOver (23+), we have a new toy: **D3DMetal**. 
This is essentially [Apple's Game Porting Toolkit (GPTK)](https://developer.apple.com/games/game-porting-toolkit/) integrated directly into CrossOver.  
Using GPTK, instead of the traditional translation path: `DirectX` -> `Vulkan` (via DXVK) -> `Metal` (via MoltenVK), D3DMetal translates DirectX headers ***directly*** to Metal.  
GPTK is a great tech, and enable some modern games to run on macOS. If you want to know more for a list of compatible games I invite you to look for [this website](https://macgametoolkit.com/).

Going back to Crossover, I enabled the D3DMetal flag it in the bottle settings, along with MSync and the performances were **much** better, despite regular stutters I investigated and actually solved (more in the next section).  

***Note:***  
For those who missed the memo, MSync replaces the old ESync/FSync methods. 
Instead of using `eventfd` (which is a Linux kernel feature that macOS lacks and must emulate), MSync maps Wine synchronization primitives directly to macOS native `kqueue` or Mach semaphores. The result is a **much** lower CPU overhead.

### The Retina Problem

However, performance was initially terrible. Why? Because macOS tells the game: "Hey, I have a 3024x1964 screen!" and the game engine says "OK!" and tries to render a poorly optimized open-world at 4K.

The fix was simple but counter-intuitive:
1. Turn **OFF** "High Resolution Mode" in the bottle settings.
2. Force the game to 1920x1080.
3. Enable **Temporal Filtering** in the game engine (which essentially renders at lower res and upscales).

Suddenly, the M3 Max was pushing stable frames. Unfortunately this "modern" approach completely breaks when you look backward.

*Watch Dogs 2* : **Success**.

## The Legacy Stack: When D3DMetal fails

I then tried to run *Tron 2.0*, a monolith classic from 2003 running on the LithTech engine (DirectX 9).

I naively kept D3DMetal on. The result? An instant crash.

Here is the technical reason: **D3DMetal does not support DirectX 9**. It currently only handles DX11 and DX12 instructions. For older games, you *must* fallback to the translation chain: `DirectX 9` -> `DXVK` -> `MoltenVK` -> `Metal`.

So, I created a separate bottle (and this is crucial: **never mix eras**).  
I set this bottle to Windows XP mode to avoid modern OS overhead, and enabled DXVK.

### The case of the missing DLL

Even with the correct graphics layer, the game wouldn't start. This is where it gets interesting for C++ developers.

The crash wasn't graphical. It was a missing dependency: `msvcirt.dll`.

This library is the *old* C++ runtime (MSVC 6.0 era). Modern Windows (and by extension, modern Wine bottles) have largely deprecated or removed it. The game was calling functions in a dynamic library that simply didn't exist in the virtual `system32` folder.

To fix this, I had to:
1.  Manually create a `Windows XP` bottle (which is more likely to handle legacy calls).
2.  Use the "Install Software" tool to inject `DirectX for Modern Games` (which oddly enough, contains legacy DX9 redistributables).
3.  Manually patch the executable with the community-made "Killer App Mod" to strip the DRM that Wine couldn't parse.

*Tron 2.0* : **Partial success**.

## The Proton Reality Check

On the four games I wanted to test only **one** launched with great perfor,ances "out of the box“. 
Two have been tweaked to run great, and only one did not run at all (due to anti-cheats software compatibility).

We often think of "emulation" (or translation, in Wine's case) as a linear performance cost. But on Apple Silicon, it's really about choosing the right translation path:

* **"Retro" Games** (<= 2012): use `DXVK` as it handles the fixed-function pipeline of older DirectX versions way better than Apple's toolkit does,
* **"Modern" Games** (> 2012): use `D3DMetal` and `MSync` provides the best combo here as it skips the Vulkan translation overhead.

However, we have to address the elephant in the room: **Proton on Linux**.

If you own a Steam Deck or run Linux on your desktop, you know that the experience is lightyears ahead of what we have on macOS. Why? Because Proton (Valve's fork of Wine) is solving a much simpler equation.

On Linux (x86_64), the CPU instructions are native. Proton only needs to translate Windows API calls to Linux Kernel calls, and DirectX to Vulkan. It's a 1:1 translation.

On macOS (Apple Silicon), the translations are more expensive:

1.  Rosetta 2 translates x86_64 instructions to ARM64 (CPU overhead),
2.  CrossOver translates Windows API to macOS API (OS overhead),
3.  D3DMetal translates DirectX to Metal (GPU overhead).

This triple-layer cake is technical wizardry, but it explains why my $400 Steam Deck can sometimes launch a game with fewer glitches (and slightly better performance) than an Apple Silicon MacBook Pro. Proton simply has less distance to cover, and Vulkan is a first-class citizen on Linux.

I actually tried to run the same four games on another machine using CachyOS with Proton and... **all four games launched without any tweak**.
That is a **major** success for Linux.

Maybe MacBooks will come to launch and run natively Windows games. **If** Apple really wants to.
