---
author: "Antonin"
title: "Old games on m1 macs, natively"
date: 2022-03-09T09:30:00+02:00
tags: ["games", "apple silicon", "apple", "macintosh", "macOS", "retrogaming"]
description: "More games to play!"
toc: false
readTime: true
---

Apple Silicon chips on the mac changed **everything**... except the gaming industry (yet).  
Even though M1 chips (M1, , M1 Pro, and M1 Max) have been sold a lot (+25% this January compared to January 2021, which had already broke all the records for mac sales), a very few companies updated their actual games to run on M1 chips.

Even if a few solutions exist to run windows games on the M1 mac, running a windows VM using [Parallels](https://www.parallels.com/fr/products/desktop/), or translating at runtime all the API calls via [Crossover](https://www.codeweavers.com/crossover), those products are expensive to run - in terms of both money and computer resources -, still labeled as "experimental", and there is a high risk to not run all your games.

I recently heard about a website that lists old games ports (think about a port as a "translation" of a game from a given architecture [x86, x86_64, powerpc, ...] to another) for mac, including arm as a target, and finally found the website: [https://macsourceports.com](https://macsourceports.com).

![](/images/mac_source_ports_2022.png#small "The home page of Mac Source Ports website")

Mac Source Ports games are **universal**, to run in both x86_64 architecture (basically an intel chip) and arm architecture (M1 chip).  
Those ports are **signed** and **notarized**, so no issue running them on your mac and no awful pop-up asking to push the application into the bin.

**But** the ports contain only the _technical_ work about the game, which **not** include the data.
So, you will have to buy the game first ([GOG](https://www.gog.com) is a great platform to buy DRM-free old games), extract the data from the official executables, and drag-and-drop the data files in the app you downloaded from Mac Source Ports.  
A software is freely available to extract the data from an executable [here](https://macsourceports.com/utilities), signed and notarized (universal binary).

There is a great list of old good games to play again and again and again:
* System Shock,
* Return to Castle Wolfenstein,
* all Quake games and all old Doom games,
* Star Wars Jedi Knight: Dark Forces II
* RollerCoaster Tycoon 2,
* Diablo,
* etc.

The website lists all known ports, and do not hesitate to contact the author of the website directly if you know a port that can be used in both x86_64 and arm architectures.

As stated by the author in the [FAQ](https://macsourceports.com/faq) section ("Why don't you have Game X?"), if the source code of the game is not available the port can't be done.
That is the only drawback of a port work.
Fortunately there are still some companies all around the world that release the source code of good old games (excluding the game data) to work on the ports, like ID software.

The full list of games is available: [https://macsourceports.com/games](https://macsourceports.com/games), and the source code of some ports is available on github: [https://github.com/MacSourcePorts](https://github.com/MacSourcePorts).
