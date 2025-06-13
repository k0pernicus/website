---
author: "Antonin"
title: "Setup video and sound for WSL2"
date: 2020-05-05T00:00:00+02:00
tags: ["wsl", "wsl2", "linux", "windows"]
description: "Linux on Windows... natively!"
toc: true
readTime: true
---

+++
draft = false
categories = ["wsl2", "linux", "windows", "development", "hack"]
disqus = false
+++

WSL, or [Windows Subsystem for Linux](https://docs.microsoft.com/en-US/windows/wsl/about), was, in my opinion, a game changer for developers, in order to run GNU/Linux utilities on Windows 10.

In 2019, Microsoft released a new version of WSL (in insiders builds): WSL2.  

WSL2 changed a lot of things: instead of emulating Linux system calls, WSL2 proposes a real Linux kernel in a light and very optimized virtual machine running on hyper-V.
This enables great file system performance (at least, better than first version of WSL), as well as full system call compatibility.

As interested by 2-in-1 devices, in order to "merge" the usages of my macbook and my iPad, I had the opportunity to test WSL2 on a Surface Pro device (6th generation) - including the Surface Pen, which is a killer-feature when you are spending your time in drawing, or annotating PDFs and taking *a lot* of notes. 

Despite the fact that I am not a Windows fan, the idea behind WSL/WSL2 was interesting and powerful enough to pass some time on, use it, and... admire the work from Microsoft, and other companies like Canonical, engineers that developed this awesome Windows feature.

Running GNU/Linux command line tools is very easy: open a terminal in your favorite distribution, run `apt-get install <your_software>`, and... that's it!
Like in any other GNU/Linux distribution, without any extra setup.

However, running windowed softwares or GUIs, sound enabled, is *far* more complicated.

**Note**: This blog post does not explain how to install WSL / WSL2 on your system and how to configure it.  
For this, I recommend you to check [the official documentation from Microsoft](https://docs.microsoft.com/en-US/windows/wsl/install-win10).

Ok, let's work on it!

## Set up the X server

### On your distro

First, we will have to get the DNS server name of your distro.
To get this, you can run the following bash command: `$(cat /etc/resolv.conf | grep nameserver | awk '{print $2; exit;}')`.
Let's name the output `DISTRO_DNS`.

To know the IP of your distro, just run `ip a` in a terminal, and check the output for `eth0`, following `inet`.
Usually, you must have something like `172.26.XXX.XXX/YY`.  
Let's put the content in a variable named `DISTRO_IP`.

In your `~/.profile` file, create the following environment variable called `DISPLAY`, and use the command to get the DNS, concatenated with `:0.0`:

```bash
export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2; exit;}'):0.0
# You can also export the previous environment variables
export DISTRO_DNS=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2; exit;}')
```

Don't forget to update your current environment (`source ~/.profile`), and... that's it for the moment :)

### On Windows

Now, it's time to run you favorite X server for Windows.  
If you don't have any preference, I recommend you to test [VcXsrv](https://sourceforge.net/projects/vcxsrv/) (which is free) or [X410](https://x410.dev/) (available in the Windows Store, not free, but a trial version is available for testing), and to configure it.

**Before to configure it**, let me explain you a thing...  
Those tools allows connections from loopback addresses (such as localhost), for security reason.
**But**, as WSL2 behaves like a VM in a separate network, it has **its own** IP address (defined in your `DISTRO_IP` variable)...  
In this case, you have to allow connections *from any device* for the X server you are using.
For Windows, it's a **public** access.

For obvious security reasons, to allow *public* access can be an issue.  
In this tutorial, I propose to set your firewall parameters in order to allow WSL2 **only** to communicate with your X server.

To configure VcXsrv, please follow [this tutorial](https://seanthegeek.net/234/graphical-linux-applications-bash-ubuntu-windows/).
For X410, you can follow [the official tutorial](https://x410.dev/cookbook/wsl/using-x410-with-wsl2/).

### Firewall settings

To setup the firewall, we need to configure the settings of your X server via the Windows firewall settings.

Type Windows key and search for "Windows Defender Settings" app.  
Now, click on "Advanced settings" to get the full settings (1), and click on "Inbound rules" (2).

![](/images/windows_defender.png#small "Windows Defender Settings (1)")

![](/images/inbound_rules.png#small "Inbound rules (2)")

You will see a big list of rules by running and non-running services, softwares, daemons, etc... and you will have to look for the name of your X server.

Once you have it, disable the rules for private profiles, and click on each public profile.

**For each public** profile for the X server, make sure that the action is set to "Allow the connection".
This enables the communication with *everything*.  
Now, click on "scope", and on "Remote IP address", click on "Add..." (3).  

![](/images/ip_set.png#small "Set WSL2 ip (3)")

In the field "This IP address or subnet", paste the content of your `DISTRO_IP` variable, click the "OK" button, and apply the settings just after.

Once you made this, you just allowed the X server to communicate with your WSL2 distribution, using it's IP.

**NOTE** Those rules have to be set *each time* that the IP of your distribution has changed :-/

Once you made the changes, open a distribution terminal, and launch `xeyes` (or anything X utility), and... tada!

![](/images/xeyes.png#small "xeyes on Windows!")

### GUIs using docker

As you set the `DISPLAY` environment variable, it is possible to run a GUI software, executed in a docker container.

Let's try this with [inkscape](https://github.com/jessfraz/dockerfiles/blob/master/inkscape/Dockerfile).  
Fortunately, [Jessie Frazelle](https://blog.jessfraz.com/) already made [great dockerfiles, including for inkscape, available on github](https://github.com/jessfraz/dockerfiles).

In order to execute an inkscape container on a GNU/Linux distribution, you can execute the following command:

```
docker run -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v /inkscape/:/workspace \
    -e DISPLAY=unix$DISPLAY \
    jess/inkscape
```

To run it on WSL2, we need here is to update the `DISPLAY` environment variable to match with ours, which is just `$DISPLAY`.  
In this case, the command to launch the inkscape container is:

```
docker run -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v /inkscape/:/workspace \
    -e DISPLAY=$DISPLAY \ # no need to prefix here with `unix`
    jess/inkscape
```

## pulseaudio

For softwares like inkscape, to not have any sound is totally fine.  
However, for softwares like Firefox, or VLC, to not have any sound associated to the video may be annoying...

Let's solve this using pulseaudio, a free multi-platform sound server.

### pulseaudio on Windows

For this part, I am making updates of a [great blog post to enable pulseaudio for WSL](https://research.wmz.ninja/articles/2017/11/setting-up-wsl-with-graphics-and-audio.html) (first version), thanks to [Mianzhi Wang](https://research.wmz.ninja/index.html).
The approach remains almost the same than WSL.

First, you will need to download the pre-built binary package from the [pulseaudio website](https://www.freedesktop.org/wiki/Software/PulseAudio/Ports/Windows/Support/), **for Windows**.  
Extract the archive, and make sure that you have four different directories: `bin`, `etc`, `lib` and `share`.  
Now, edit the file `etc/default.pa`, to replace the line 61 by:

```
load-module module-native-protocol-tcp auth-ip-acl=<DISTRO_IP> auth-anonymous=1
```

**and replace `<DISTRO_IP>` by the content of you `DISTRO_IP` variable**.

To finish, in `etc/daemon.conf`, replace `exit-idle-time = 20` to `exit-idle-time = -1`, in order to turn off idle timer.

You can now run the pulseaudio server launching `pulseaudio.exe`, contained in the `bin` folder.

### pulseaudio on your distro

On your distro, you can first purge the existing pulseaudio build and settings, and build & install the latest pulseaudio on your distribution like this:

1. `sudo apt-get purge pulseaudio pulseaudio-utils`
2. Install version 13 of pulseaudio: `wget https://freedesktop.org/software/pulseaudio/releases/pulseaudio-13.0.tar.xz`
3. Uncompress the archive in order to configure and install it: `tar -xvf pulseaudio-13.0.tar.xz && cd pulseaudio-13.0`
4. Now, configure and install:

```bash
CFLAGS="$CFLAGS -g -O0" ./configure     \
    --disable-bluez4                    \
    --disable-bluez5                    \
    --disable-rpath                     \
    --disable-asyncns                   \
    --disable-udev                      \
    --disable-systemd-daemon            \
    --without-caps                      \
    --enable-force-preopen && make -j 4 && sudo make install
```

pulseaudio might be install on `/usr/local/`.

Now, we will need to set another environment variable to let pulseaudio to run over TCP, in your `~/.profile` file:

```bash
export PULSE_SERVER=tcp:$DISTRO_DNS
```

Once you did this, let's configure the firewall, as we did previously with the X server.

### Firewall settings

Like the X server part, you will need to set the rules via the Windows firewall settings.

Type the Windows key and search for "Windows Defender Settings" settings.  
Now, click on "Advanced settings" to get the full settings, and click on "Inbound rules".  
In the list, check for `pulseaudio` rules, and disable each private rule for this specific software.

**For each public** profile for `pulseaudio`, make sure that the action is set to "Allow the connection".
This enables the communication with *everything*.  
Now, click on "scope", and on "Remote IP address", click on "Add...".  
In the field "This IP address or subnet", paste the content of your DISTRO_IP variable, click "OK", and apply the settings.

Once you made this, you will be able to hear music and sound from WSL2 running softwares, on your Windows machine!

## Conclusion

Using those rules, you will be able to get video and audio from your WSL2 distribution, on your Windows machine.  

I am sure we can automatize a lot of stuff here, including the fact to retrieve automatically the IP of the distribution, and update dynamically the firewall settings and pulseaudio settings... but I don't know how to write scripts for Windows.  
If you have any idea about automation, please let me know in the comments, using Twitter, or by email :)
