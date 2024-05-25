+++
title = "The Terrible Situation of Windows (Professional) Laptops"
date = 2024-05-19T17:25:00+02:00
draft = false
categories = ["thoughts", "windows", "hardware"]
tags = ["thoughts", "windows", "hardware"]
description = "Shut Up and (do not) Take my Money!"
disqus = false
+++

Last Saturday, after logging on ~~Twitter~~ X, I came across two tweets from two great programmers I follow.

{{< tweet 1791925515281899810 >}}

{{< tweet 1791437076455411888 >}}

**Context**: Jonathan Blow and Omar Cornut are two programmers focusing on games and game engines 
development.

**Note**: I will use the word "professional" for laptops with high-end specs and components like CPUs, 
GPUs (which includes "Gaming" laptops), and RAM, and big battery capacity.

In the first tweet, Jonathan Blow exposes its troubles on its latest professional laptop:
1. the laptop lasts around one year before dying ✝,
2. the laptop suddenly starts from itself in his laptop, got hot, and runs into battery life issue(s),
3. the audio does not work well, with terrible lagging and audio skipping,
4. CPU / GPU performances become weak at very random times,
5. and the SSD is "*excruciatingly* slow" when the laptop is unplugged.

In the second tweet, Omar Cornut exposes also the fact that its latest Windows laptop does not follow
the same rate of performance / price compared to an half-priced Apple macintosh.

Buying a Windows laptop (or just computer hardware in general) there are two different things to differenciate: 
1. the hardware and its manufacturer, 
2. and the operating system shipped with the laptop, coming from Microsoft.

and we can rearrange the list of complaints of the first tweet based on this.

The list of the laptop manufacturer faults:  
* bad hardware quality (including SSD),  
* shitty cooling system,  
* not focused on real-usage quality & tests,  
* high pricing for shitty hardware.

The list of the Microsoft Windows faults:
* the hibernate system that does not seems to work for laptops,  
* inexplicable performance drops,  
* calculating superficial things in background, which explains the wake-up in the laptop.

Both seem to fault here, and we can ask ourselves how evolved the professional laptops "industry" since the last 10 years.

**Note**: the title of the next sections can contains spoilers about each evolution.

## The greediness of the laptop manufacturers

Today, the based price of a professional laptop is between 1000 and 1200$.  
As an example, if you go to the website of the famous DELL manufacturer, you can find some XPS devices with latest 
Intel chip in it, at least 16GB of RAM, 512GB of SSD, but no dedicated graphics card.  
If you count on a GPU, then you will have to upgrade some specs (like the size of your laptop and, then, the size of 
your screen) to have a basic NVIDIA RTX 4050 mobile GPU in a 16" XPS for 2500$. 

4000$ for a laptop is a thing. And for this price you should get the _Rolls-Royce_ of the modern laptops with:
* best hardware quality as possible,
* best customer service as possible,
* and best overall experience as possible.

So, in total, we should expect for a device that costs 4x the price of a "basic" professional laptop 
**the best reliability ever**.

But how can we compute that reliability?

As for smartphones, we can count like that:
1. best overall quality of each component, and its homogeneity,
2. if the manufacturer shipped drivers for Windows only or for GNU/Linux also,
3. the software support in terms of drivers,
4. the battery life.

Among the best selling devices for professional we can count on many different brands.
I will only take care of four different here for my expose, and a recent one called "Framework".

#### The quality of the components

Professional laptops contain their own lot of good, bad, and weird components.
The Gaming category contains maybe the most non-sense use of components among the different categories of laptops.

As an example you can find 4K panels on 14" screens, which consumes a lot of battery and is not very well different 
than a 2K panel for this screen size. However the "4K screen" ad bumps up the price of the hardware.
As a personal opinion, I would prefer a 1080p or 2K OLED panel than a bad calibrated 4K panel... especially on a 14" 
device. 

RGB lighting is another example of "flashy" laptops that rely mostly on its design.
Except if you want to show off during a Twitch session, I don't think this is what you really want to show working in
a coffee shop on Monday morning...

Slim designs are a weird thing for Gaming laptops, as they need to cooldown heavily to avoid throttling.
Slim designs reduce the space needed for components, increase the price of each component too as they have to be
specifically built for a laptop model specifically, and also diminishes the repairability of the laptop.
In a funny way, the most expensive laptops seem to be the slimest devices - so, more you spend money on a device and
less you care about reparaibility...

Finally, the most stupid idea are coming from Razer to introduce a high-end expensive mobile GPU with a 140W power with
a low TDP and power supply that **can't** fully power it... So, you pay for the high-end mobile GPU and you only obtain
the same power than a chipper GPU card.
Razer is not the only manufacturer to blame here: the same thing happened since years about the CPU, where the cooling system was
not good enough to avoid general throttling, which reduces drastically the performances of your laptop.

{{< youtube id="DEEc65x4SMs" autoplay="false" >}}

Talking about cooling systems, innovations about cooling systems stopped since years now, and manufacturers usually 
install a larger fan and a more agressive "Extreme Performance" software profile (only available when plugged).

And, finally, the CPU.
CPUs had a really low increase in terms of performance on Windows laptops but are, at least, as expensive as
previous generations of CPUs.
They consume more and more energy to try to performance better than the previous generation but sometimes fail because
of bad thermal performances and throttling.
You can check the prices of new high-end laptop CPUs [here](https://www.cpubenchmark.net/laptop.html), which are mostly
in the same range of prices than a general business laptop.

#### OS support

Windows 10 has been introduced in 2015, and its end is scheduled for 2025.
A majority of Windows 10 laptops, introduced in 2015, where **not** suitable to move to Windows 11, so most of those
will just die once the EOL of Windows 10 will be claimed.

Windows 12 specs will tell us if Microsoft is forcing people to buy new hardware, or if Windows 11 was only a 
"step for security" as they claim.

Except DELL no one supports GNU/Linux for hardware drivers, and DELL only does this for its XPS brand, no other.

Another thing is the number of bloatwares shipped with a laptop today, and the number of background services that
are running to ask you to register somewhere for whatever reason...
Those applications are really annoying and come with plenty of default softwares from Windows 10/11 that slow down
the overall performances of your computer.

#### Drivers support

About laptop lifespan per brand, it is very difficult to have reliable information from those manufacturers.
Despite searches I only found that Microsoft released [on official statement](https://learn.microsoft.com/en-us/surface/surface-driver-firmware-lifecycle-support) about 4 to 6 years of software support, 
at least, depending of your Surface device.

However, 2 years ago, a Medium user called "Greencodetech" has done its own searches on the subject and published a 
paper about it [here](https://medium.com/@greencodetech810/average-laptop-lifespan-by-brand-128c11394dd3).

Its own searches led him to this top 5 (according to hardware models):
1. Lenovo, with 5 to 7 years of drivers support,
2. DELL, with 5 to 6 years of drivers support,
3. HP, with 5 to 6 years of drivers support,
4. Microsoft, with 4 to 6 years of drivers support,
5. Acer, with 3 to 4 years of drivers support.

#### The battery life

A laptop without battery life is just as useful as a car without wheels: a theoritical portability.

Most professional laptops support a "good" battery component, drained very quickly by the energy-consuming components 
that compose the laptop.  
To avoid your battery life to end in less than 2h, Microsoft and laptop manufacturers imagine different profiles to
help components to consume less energy while reducing their performances.
So, as a result, your computer will lasts 4h but the compilation time will lasts 2x more than plugged...

Another fact is some components will be **disabled** on purpose to avoid waste of energy... so, as the manufacturer did
not increased the capacity of your battery life (or just did not invested on improving the situation here) you will
have a decent battery life but with weaker performances and disabled hardware parts.

#### In overall...

Professional laptops are as expensive as previous generations, but they are, at best, as "ok" as the previous generations,
if not weaker.

The OS and drivers support is in average 5 years **but**, if I am being honest here, I never found a person with a 
5-years professional laptop in its hands since a while, mostly because the device died by itself, or some components 
stopped working well, or just the need to upgrade to better specs, which is not what you can do easily on modern 
professional laptops.

The fact that certain brands like Razer, DELL, or Microsoft let you buy very expensive laptops with expensive components
on it that you can't use at full speed and / or at full power is another fact that end-users are not respected.

#### A possible future : the Framework laptop

One **possible** alternative is called [Framework](https://frame.work), which focuses very heavily on scalable and modular
configurations, and long-lasting hardware.  
Unfortunately there is still no reliable data to know how really lasts a Framework laptop, and how reliable those modular
configurations perform.

## The unreliable state of Microsoft Windows

How can we explain that our laptops have more powerful components than 10-15 years ago but software still feels "laggy"?  
A simple hypothesis is our computing devices are actually busy to execute shit instead of focusing only 
on user's demands.

It is not a surprise that each new version of Windows from Microsoft is a fiasco.  
However, the new mainstream thing - since Windows 10 actually - is that each new update of Windows 
introduces again [more bugs](https://learn.microsoft.com/en-us/windows/release-health/status-windows-11-23h2) 
(and more ways to lose your data), less useful features, more ads, and less performance!

As an example, in 2022, Tom's Hardware performed [a benchmark](https://www.tomshardware.fr/windows-10-vs-windows-11-quelles-performances-pour-un-pc-classique) on running the exact same software on a classical machine running 
two different versions of Windows (10 and 11).
Tom's Hardware found out that each software was running between 6% and 10% **slower** on Windows 11 than Windows 10.  

The Microsoft marketing team does seem aware of this issue as they stated, releasing Windows 11:
> Both operating systems have similar security features, update processes, and compatibility with hardware and software. 
However, **Windows 11 offers improved performance and features compared to Windows 10**, making it a better choice for those looking for a more modern and efficient operating system.
To get this statement, please follow [this official Microsoft webpage](https://techcommunity.microsoft.com/t5/windows-11/windows-11-vs-windows-10-a-comprehensive-comparison-of-features/m-p/3760255).

Also, Microsoft did not even tried to fix one of the most noticed bug on a laptop for Windows 10 and Windows 11: 
"modern standby", which leads to a Windows laptop that wakes up suddenly in your backpack.
This is the issue Jonathan Blow stated in its tweet.  
This issue has been raised by everyone, including all modern streamers and YouTubers, like Linus Tech Tips in the following 
video. 

{{< youtube id="OHKKcd3sx2c" autoplay="false" >}}  

Microsoft acknowledged the issue, and released several fixes that do not seem to work for everyone, 
as this bug is regularly reported again on some official and unofficial Windows forums.

#### Is there an OS alternative for gamedevs ?

As I said earlier the two tweets authors are working in game and game engine development, and I understand why both
 authors are targeting Windows as a default / main platform for their work, despite the weird state of this OS today and
 the macOS and GNU/Linux alternatives today.  
First, as a game developer you may want to target the largest community of gamers as possible, and this community is 95% on... Windows.  
Then, as a game developer you may want to target also the console gamers, on Xbox, PS5 and Nintendo Switch.
The main issue is that all tools from those console manufacturers, including the SDK, are exclusively built for... Windows.

Unfortunately, to target the main audience in gaming (industry / market), **there is no big room for GNU/Linux or macOS**.

{{< youtube id="llK5tk0jiN8" autoplay="false" >}}

<!--

## The main question

Let's step back for a minute, and ask ourselves some questions.

*Since at most 10 years, did the battery life of our powerful professional Windows machines have been improved?*  
Definitely no.  
If you have your computer unplugged you will likely limit your battery settings to "NORMAL" or "LOW ENERGY", which 
will drastically reduces the performances of your machine, otherwise the computer will not lasts more than 2h most of the time.

*Since at most 10 years, did the performances of our powerful professional Windows machines have been improved?*  
Yes! But the ratio Price / Performance is even more disturbing than before...

*Since at most 10 years, did the laptop hardware improved (better screen panel, incredible keyboard and glassy high-precision touchpads)?*  
It depends of the manufacturer, and the price you had to put on the device.  

So, based on all those information, **what is the purpose of a powerful Windows laptop nowadays?**  
And **do Microsoft Windows and laptop manufacturers care about professionals who want performance and mobility?**

-->
