+++
title = "The state of Vulkan apps in 2024"
date = 2024-05-28T20:30:00+02:00
draft = false
categories = ["vulkan", "graphics", "graphics APIs", "graphics programming"]
tags = ["vulkan", "graphics", "graphics APIs", "graphics programming"]
description = "The Vulkan did not explode (yet?)"
disqus = false
+++

As a game engine developer, I always ask myself the same question working on new features or bug fixes: "**What are the targets of my game engine?**".  
Excluding the consoles, this question mostly implies which **operating system** - or platform - is targeted: Windows, 
Linux (or GNU/Linux, but I will keep "Linux" for the rest of the article), or macOS.

Developing the same game on multiple platforms could increase the developer's interest to use the **same** 
technology for the different platforms.
This may reduce the development time and be more productive.

Targeting multiple platforms among Windows, Linux, or macOS, only two mainstream graphics APIs might be useful:
 **OpenGL** and **Vulkan**.
I exclude WebGPU on purpose here as the API is not stable yet.  
OpenGL and Vulkan have their specifications developed by the same company, Khronos.
However Khronos declared in 2017 that [OpenGL is no longer in active development, in favor of 
the Vulkan API](https://www.khronos.org/news/archives) where the first stable version was released in 2015.

So, eight years after its introduction, what is the state of Vulkan today among graphics apps?  
Is Vulkan finally a "DirectX 12 killer", or is it (unfortunately) a splash in the water?

## A retrospective

Vulkan has a [nice community-based list of released apps using its own API](https://www.vulkan.org/made-with-vulkan).
The main issue of this list is it is non-exhaustive, as some Vulkan apps may not belong to this list if the game developer(s), 
or the community, did not suggested it.  
I would like to thanks Khronos to publish this list and allow the community to contribute.
This list made my research easier.  
Also, I invite you to suggest **any app** that uses Vulkan but not listed!

**Note**: a Vulkan app is not necessarily a game, but any software that uses the Vulkan API 
in its backend (for example: `geekbench`).

201 Vulkan apps have been released since 2015 (202 listed apps, but one duplicate one called "`Golf It`").

Using those data I will draw three different graphs, and three different conclusions:
1. the number of released Vulkan apps since 2015, year by year, compared to DirectX 12,
2. the number of released Vulkan apps per platform, and the number of platforms targeted,
3. and the number of released Vulkan apps per SDK version.

### Released Vulkan apps per release date

As we can see below, the number of apps using Vulkan is decreasing since 2019, from 35 released apps in 2022 to 
9 released apps in 2023.

![](images/vk_released_games_per_year.png)

Based on the Wikipedia page ["Game Engines that support Vulkan(API)"](https://en.wikipedia.org/wiki/Category:Game_engines_that_support_Vulkan_(API)) 11 game engines support Vulkan as a graphics API, most of them from very famous video game companies like [id Software](https://en.wikipedia.org/wiki/Id_Software), 
[Valve](https://en.wikipedia.org/wiki/Valve_Corporation), and [Epic Games](https://en.wikipedia.org/wiki/Epic_Games).  
In my knowledge only [Id Tech 7 supports Vulkan rendering **only** for PCs](https://en.wikipedia.org/wiki/Id_Tech_7).

As a comparison:
* [PCGamingWiki shows more than 1600 games released since 1997 using OpenGL](https://www.pcgamingwiki.com/wiki/List_of_OpenGL_games), **16 in 2023** despite the fact that no new OpenGL version has been released since 2016, **which is more than with Vulkan in the same year**,
* [PCGamingWiki shows more than 430 games released on the latest version of DirectX](https://www.pcgamingwiki.com/wiki/List_of_Direct3D_12_games) (DirectX 12 and DirectX 12 Ultimate) since 2015, **88 in 2023**.

So, **twice** more **games** have been released with DirectX 12 than Vulkan in total, despite the fact that DirectX 12 is 
exclusive to Windows.

![](images/vk_released_games_per_year_vs_dx12.png)

As we can see here, the number of released Vulkan apps, compared to the number of released DirectX 12 apps, was really close
 until 2021.
Once DirectX 12 Ultimate as been released in 2020 we can clearly see a winner with DirectX 12.  
Multiple factors can be explained here: the introduction of the Xbox Series S|X in 2020 and the fact that DirectX 12 targets
also Xbox One & Xbox Series consoles (Microsoft **does not** allow any graphics API other than DirectX 12 on Xbox Series, not even
DirectX 11), the fact that most AA and AAA games are released after 3-4 years of development 
(and the introduction of DirectX 12 in new engines), or maybe the fact that DirectX 12 is good enough to target 
the platform with most users on it: Windows.

### Released Vulkan apps per platform

Based on the previous observation, we can ask ourselves whether the demand for cross-platform applications is high or negligeable, 
and what are the main platforms that Vulkan apps developers are targeting.

![](images/vk_released_games_per_platform.png)

Windows and Linux are clear winners for Vulkan since 2015, with 41.7% of Vulkan apps for Windows, and 30.2% for Linux.
The Android mobile platform is kind of a success compared to iOS, with 16.2% of Vulkan apps.  
Unlike Sony and Microsoft consoles, the Nintendo Switch officialy accepts Vulkan as a graphics API for first and third-parties games.

To finish, the major Vulkan apps targets **only one platform**, which may be a big disappointment 
given Vulkan's promise to be multi-platforms.

![](images/vk_released_games_with_platforms_support.png)

### Released Vulkan apps per API version

Now, let's take a look at which API version developers are using: do they prefer the later APIs or do they stick with
the first versions of Vulkan instead?

![](images/vk_released_games_per_vk_version.png)

This graph is not easy to interpret as there is a **large** number of Vulkan apps that does not expose publicly their 
API version.  
However, **if we exclude the 36.8% of unknown versions**, we can notice that developers did not
move to a SDK version superior at 1.1 to release their Vulkan apps, **which has been released in 2018**.  
Vulkan 1.2 has been introduced in January 2020, more than four years from now, approximately at the same time as 
DirectX 12 Ultimate.

## The 2023 survey

LunarG organizes a survey about Vulkan by Vulkan developers, since a few years now.
The survey is free, open for everyone, and LunarG published the 2024 results [here](https://www.lunarg.com/wp-content/uploads/2023/04/2023-Ecosystem-Survey-Public-Report-06APR2023.pdf).

Despite the fact that the survey has been largely shared to Vulkan developers (more than 13.000+ recipients
of the LunarG Vulkan SDK mailing list, social networks, Vulkan Slack channel, etc.) only 275 developers answered 
the LunarG's 2024 Vulkan survey.
In comparison there were 288 respondents in 2022, and 364 in 2021.

As stated in the survey the main targets of the recipients are: 
1. Windows (11, 10), 
2. Linux,
3. Android.

This reflects our observation that the great majority of released apps is actually for Windows and / or Linux 
platforms only.

Vulkan apps developers do not seem to target macOS (yet?), and state that targeting Android can be an issue as the
 driver support is still "poor".  
 However there seems to have more interest in supporting Vulkan on Android than iOS, despite the efforts of a dedicated
 Khronos team to port Vulkan on Apple platforms with [MoltenVK](https://moltengl.com/moltenvk/).

To finish, developers show once again that the complexity of the Vulkan API is still a barrier to adoption, 
despite the fact that it is now 8 years old.  
This statement can be a surprise as DirectX 12 is criticized too for its complexity compared to DirectX 11, **but** still
gains traction from the Gaming industry compared to Vulkan.
