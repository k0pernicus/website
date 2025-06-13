---
author: "Antonin"
date: "2016-09-01T13:41:55+02:00"
description: "An approach to assess and improve the energy consumption of Android apps"
tags: ["android", "internship", "hotpepper"]
title: "The Hot-Pepper approach"
toc: false
readTime: true
---

_Note: These observations are reported in my research report [here](/m2-mocad-internship.pdf), and a research paper is currently drafted on this. The source code of this approach is visible in [Github](https://github.com/SOMCA)._

During my final internship at [LATECE](http://www.latece.uqam.ca) in Montreal, I developed an approach, supported by a Python framework, to evaluate the energy consumption of Android bad practices, using the source code of the app.
This approach is called _**Hot-Pepper**_.
_**Hot-Pepper**_ helped us to find that 3 popular Android code smells may be considered as energy code smells: _HashMap Usage_, _Internal Getter/Setter_ and _Member Ignoring Method_.
Also, it permits to evaluate picture practices, in terms of energy consumption, like formats (PNG vs JPEG), picture size (original JPEG and compressed JPEG, without degrading the quality of the picture) and picture bitmap formats to use (ARGB\_8888 vs RGB\_565).

Using this approach, we improved the energy consumption of 5 popular Android apps.

The approach is automated and really easy to use.
It required:

* the original Android source code of the app,
* a smartphone plugged to a measuring device (ammeter, voltmeter, ...),
* a scenario based on user events ([Calabash](http://calaba.sh), [Robotium](http://robotium.com), ...).

The output of this approach is the source code of the most energy-efficient Android app, and the associated APK to deploy it immediatly in popular app stores.

_**Hot-Pepper**_ consists in 3 steps:

1. detect and correct of Android bad practices,
2. compute energy metrics,
3. analyze energy metrics.

The first step uses an improved version of _**Paprika**_, a Java application to detect statically Android code smells in an Android APK (if you are interested in using this tool, you can contact Geoffrey Hecht [here](mailto:geoffrey.hecht@inria.fr), the creator of _**Paprika**_, to obtain an access).
The two last steps use _**Naga Viper**_, a software that compute and analyze dynamically some energy metrics, collected on an Android app.
I developed _**Naga Viper**_ and some Python libraries during this internship, and it works well!

I am somewhat disappointed to not have developed _**Naga Viper**_ using anything else than Python (always better than Java but...) and to not have deploy the framework in the cloud.
First, I wanted to use [**Rust**](https://www.rust-lang.org) or [**Golang**](https://www.golang.org) to implement _**Hot-Pepper**_.
But many students don't know these programming languages, and are not interested in understanding the advantages of those languages for this project (do you feel the frustration in my words?).
The second thing is that my demand to implement the framework with a new language has been understand, and that the framework will be deploy in the cloud after the new version in... Java (-_-).

Also, I tried to develop a tool, called _**Ghost Pepper**_, which develop automatically interesting scenarios using user events, for a given Android app.
The development of this tool has been more problematic than previously thought, especially to maintain a good ratio between the exploitation and the exploration of the app (the tool uses a genetic algorithm to create a new scenario), and has not been fully tested to use _**Hot-Pepper**_ on selected apps.

_**Hot-Pepper**_ takes me less than 4 months to have some accurate and repeatable results.

Even if it works well, the approach may be improved, and I hope Android developers will use this approach to care about the energy consumption of their apps.
