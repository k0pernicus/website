---
author: "Antonin"
title: "Bringing Swift through Distrobox on nixOS"
date: 2026-06-28T20:00:00+01:00
draft: false
description: "A quick method to bring Swift on an unsupported linux distribution"
tags: ["nixOS", "swift"]
readTime: true
---

I am working on a [project that combines Swift, Raylib, and Raygui](https://git.carette.xyz/k0pernicus/recreational-swift-raylib-game).  
I don't really know what is the end-goal here, but I have fun. That's enough for me to work occasionally on this thing.

I like Swift, very much, and I got more fun developing in Swift than Rust or C++ recently.

But, there is a problem with Swift: its adaptability to Linux distribution.  
Even if Swift is a general programming language, its official support does not include nixOS, a Linux distribution I use since a few months now.

The latest version of swift, maintained by the nixOS community, is the [5.10.1](https://search.nixos.org/packages?channel=26.05&query=swift#show=swift) one.  
People are actually trying to bring the latest version of Swift to nixpkg, but [it is not ready yet](https://github.com/lucasly-ba/swift-nixos) and the installation process today is still cumberstone.

I wish I would have more time and knowledge to contribute, but I also wanted to test very quick my project on another machine than my mac.

So... [distrobox](https://github.com/89luca89/distrobox) to the rescue!

## The configuration

Distrobox is a pretty cool project.  
It's main objective is to bring any Linux distribution inside your terminal, via containers. 
Compatibility is great, and the created container will communicate tightly with the host, sharing the HOME directory of the user, graphical apps (X11, Wayland), audio, ... which is absolutely perfect for my needs!

For containers, I use rootless ones, using `podman`:

```nix
virtualisation.podman = {
  enable = true;
  # This makes typing 'docker' under the hood use podman
  dockerCompat = true; 
  defaultNetwork.settings.dns_enabled = true;
};
```

and then I enable `distrobox` in my global environment:

```nix
environment.systemPackages = with pkgs; [ distrobox ];
```

After a quick system rebuild, I can finally setup my distrobox environment for Swift:

```sh
distrobox create --name swift-env --image ubuntu:26.04
```

Even if Swift is compatible with ubuntu:24.04 I prefered to use the 26.04 LTS version instead.  
This would need an extra step as a dependency uses another version of the one provided by apt.

Now, let's enter the ubuntu container:

```sh
distrobox enter swift-env
```

and... that's it! I am in an ubuntu environment, on my nixOS machine. Great!

Now, let's setup that container like if it is a true local ubuntu system:

```sh 
sudo apt update &&
sudo apt install -y mesa-utils x11-apps git make libxml2-dev && \
curl -O https://download.swift.org/swiftly/linux/swiftly-$(uname -m).tar.gz && \
tar zxf swiftly-$(uname -m).tar.gz && \
./swiftly init --quiet-shell-followup && \
. "${SWIFTLY_HOME_DIR:-$HOME/.local/share/swiftly}/env.sh" && \
hash -r
```

Now, `swiftly` and the Swift programming language are installed in that ubuntu container.

I can now go in my local project, and try to run it (debug mode):

```sh
cd ~/Devel/recreational-swift-raylib-game && swift run -c debug
```

and...

```sh
/home/user/.local/share/swiftly/toolchains/6.3.2/usr/bin/lldb: error while loading shared libraries: libxml2.so.2: cannot open shared object file: No such file or directory 
```

This makes sense actually.  
As you might reminder, I said earlier that the Swift installer proposes ubuntu:24.04 version, but not the 26.04 one. 
In this newer ubuntu release, the `libxml2` developers introduced a backwards-incompatible update that bumped the shared library name from `libxml2.so.2` to `libxml2.so.16`.  
However, the pre-compiled Swift toolchains available through swiftly were built in environments that **strictly expect the older `libxml2.so.2`**. So, even though we have the latest version of the library installed, `lldb` is completely blind to it.

Let's use the good old hack to trick the Swift toolchain into using the new version of the library by creating a symbolic link from the old filename to the new one:

```sh
sudo ln -s /usr/lib/x86_64-linux-gnu/libxml2.so.16 /usr/lib/x86_64-linux-gnu/libxml2.so.2
```

and... it works!

### The distrobox ini file

To automatize that process each time, `distrobox` allows to create an `ini` file that process automaticall things for you:

```ini
[swift-env]
image=ubuntu:26.04

init_hooks="apt-get update"
init_hooks="apt-get -y install mesa-utils x11-apps git make libxml2-dev"
init_hooks="curl -O https://download.swift.org/swiftly/linux/swiftly-$(uname -m).tar.gz"
init_hooks="tar zxf swiftly-$(uname -m).tar.gz"
init_hooks="./swiftly init --quiet-shell-followup"
init_hooks="ln -sf /usr/lib/x86_64-linux-gnu/libxml2.so.16 /usr/lib/x86_64-linux-gnu/libxml2.so.2"

# Pull the latest version of the image if it isn't cached
pull=true
```

## A praise to nixpkgs maintainers

Even through this quick hack is great to test quickly a build on a linux system, it does not fit the tradition of nixOS: automatic deterministic system.

I want to thanks all contributors and maintainers of the Swift programming language for nix for their work and the time they pass to bring this (great) programming language to nix systems.

So, a big thanks to Daniel Duan, Samasaur, Stéphan Kochen and Tom Repetti for maintaining Swift for nix systems. Also, thanks to [Lucas Ly Ba](https://github.com/lucasly-ba) for its contributing for [bringing a new way to build Swift from source on nixOS](https://github.com/lucasly-ba/swift-nixos).

I plan to pass more time on contributing as well on that topic, and on LLVM side (trying to contribute to Swift WASM), as soon as my free time will increase.  
But, for now, this hack works for me.
