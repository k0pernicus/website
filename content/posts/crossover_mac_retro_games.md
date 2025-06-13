---
author: "Antonin"
title: "Gaming on mac in 2023"
date: 2023-12-26T12:00:00+01:00
description: "A crossover love story"
tags: ["games", "apple silicon", "apple", "macintosh", "macOS", "retrogaming"]
readTime: true
---

If you know me since a while you know my love for the macintosh and the Apple Silicon chips.
Actually my main daily driver is an Macbook Air M2 and I have a Steam Deck next to me which I bought to play Windows games in an handheld format.
But, sometimes, I just want to play "old" games on the mac device, which may not be compatible with macOS x86/64, so no Rosetta 2 translation available...

In a [post last year](/posts/mac_source_ports) I wrote about **mac source ports**, an excellent website that lists all games and game engines ports from Windows to macOS.
This time, as I have little free time, I tried out the new version of [crossover](https://www.codeweavers.com/crossover), that I bought in a Black Friday discount (thanks [Andrew Tsai](https://www.youtube.com/@Andytizer)!) after checking sales on GOG and looking for a good game to play on.  
After a few searches I found out that one of my favorite games on Xbox 360, [**Stranglehold**](https://en.wikipedia.org/wiki/Stranglehold_(video_game)), was on sale at 2 euros!
I haven't played this game for years and I was wondering how this game would run on CrossOver... so I bought it, reinstalled CrossOver on my mac and tried out to create a bottle from scratch to play this game on my Macbook Air.

First I did a quick search in the CrossOver database and I found out that the game has been rated 4/5 by some users, nice!

![CrossOver page of Stranglehold](/images/stranglehold_gog_3.png)

Unfortunately the bottle is using the Steam app as a backend, and I don't want to have a static Steam application for each of my games.
Also I just bought the game on GOG so...

So, first, I created a new bottle called "Stranglehold", and I downloaded the files from GOG in a dedicated folder.

![GOG page of Stranglehold](/images/stranglehold_gog_1.png)

![Finder view of Stranglehold folder](/images/stranglehold_gog_2.png)

The files are:
* one binary, that contains an installer for GOG Galaxy, and look for the files,
* and the .bin files that are basically disk images containing the install data.

***Note:***
_
If you want to look at all the data files you can download [`innoextract`](https://constexpr.org/innoextract/#download) and just run `innoextract -l <GAME.EXE>`.
An alternate tool is [`Extractor`](https://www.macsourceports.com/utility/extractor) but it is only available for macOS.
_

Just link the executable in the folder to the bottle, and the installation process will be executed automatically, **very nice**!

CrossOver exposes a lot of configuration settings for a bottle, and you might be interested by some of them depending of the game your are running on.
Basically D3DMetal supports translation to DX12 and DX11 (kinda). 
DXVK, in the other hand, can somewhat support DirectX versions between DX9 and DX11 (included).  
Stranglehold was developed using Unreal Engine 3, using DirectX 9.0, so using DXVK can be a good option to have better performances.    

![Stranglehold bottle view](/images/stranglehold_gog_4.png)

If you want to display some additional informations about how well the game is running, you can go to the game's C drive, find the `cxbottle.conf` file at the root of the bottle and add the following line to it:
```bash
"DXVK_HUD" = "fps,frametimes,gpuload"
```

![Stranglehold bottle view](/images/stranglehold_gog_6.png)

At the end I could enjoy Stranglehold on my mac, running without any issue!
