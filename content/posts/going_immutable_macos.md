---
author: "Antonin"
title: "Going immutable on macOS"
date: 2025-12-31T14:30:00+02:00
draft: false
description: "From imperative chaos to nix-darwin"
tags: ["macos", "darwin", "nix"]
toc: true
readTime: true
---

With no surprise the end of one year marks the start of the next.  
And the beginning of a year is always synonymous with... a fresh macOS system!

But managing a good working environment on macOS has long been a game of "hope for the best." We’ve all been there: a `curl | sh` here, a manual `brew install` there, and six months later, you’re staring at a broken `PATH` and a Python environment that seems to have developed its own consciousness.

I’ve spent a lot of time recently moving my entire workflow into a declarative system using nix. From my zsh setup to my odin toolchain, here is why the transition from the imperative world of [Homebrew](https://brew.sh/) to the immutable world of [nix-darwin](https://nix-darwin.org/) has been both a revelation and a fight.

## The problem to solve: imperative rot

Homebrew is great.  
From the Linux world, it was the perfect missing package manager for macOS at first, promoted by a lot of developers, and it (mostly) works.
But Homebrew has a problem: it is imperative. 

When you run `brew install` **you are changing the state of your machine** in a way that is difficult to reverse or replicate exactly.  
As an example, if I set up a new Mac today, `brew install neovim` might give me version 0.10. However, if I do it six months from now, the version might changed and I had to reconfigure some components because of that change. The consequence is me, spending a few hours debugging my environment instead of writing code.

A solution to that is postfix every brew package by its version, but this is not possible for every package.

Another solution is system immutability and the Nix store.

## System immutablility, and the Nix store

[Nix](https://nixos.org/) approaches the problem from a functional programming perspective.  
Your system is not a collection of side effects, but a **pure function** of your configuration.

This allows different strong points like:

* **reproducibility**: Every package lives in `/nix/store` with a unique hash. This means I can have three different versions of the Odin compiler side-by-side, and they will never see each other, or even mess up with the different versions.
* **rollbacks**: If a system update breaks my shell, I don't panic. I just boot into a previous generation. The previous state of my system is still sitting in the store, untouched.
* **flakes**: Using `flake.lock`, I pin my setup to specific git commits. If it works on my laptop then **it will work** on yours. Period.

Rollbacks on macOS exist, and are great to go back to a previous version "that works".  
As an example, to list all my previous generations, I can do:

```sh
> sudo nix-env --list-generations -p /nix/var/nix/profiles/system
   1   2025-12-28 01:21:38
   2   2025-12-28 01:33:47
   3   2025-12-28 09:34:27
   4   2025-12-28 09:43:15
   5   2025-12-28 11:00:49
   6   2025-12-28 11:10:12
   7   2025-12-28 11:19:03
   ...
```

Each generation is also stored in a read-only specific volume on your system, which prevents accidental mutability of your packages.

On macOS `nix-darwin` allows me to modify everything, from Finder settings to the Dock, or the Trackpad.

As an example, this is a part of my setup:

```nix
...
configuration = { pkgs, ... }: {
    # Firewall settings
    networking.applicationFirewall = {
        enable = true;
        enableStealthMode = true;
        allowSigned = true;
        allowSignedApp = true;
    };

    system.defaults = {
        CustomUserPreferences = {
          # Disable siri
          "com.apple.Siri" = {
            "UAProfileCheckingStatus" = 0;
            "siriEnabled" = 0;
          };
          # Disable personalized ads 
          "com.apple.AdLib" = {
            allowApplePersonalizedAdvertising = false;
          };
        };

        # Show battery percentage in the menu bar
        controlcenter.BatteryShowPercentage = true;

        # Allow touch to click
        trackpad.Clicking = true;

        # Hide the dock after a small delay of inactivity
        dock.autohide = true;
        dock.autohide-delay = 0.25;

        ...
    };
}
```

And all those settings are documented in the official nix-darwin manual [here](https://nix-darwin.github.io/nix-darwin/manual/).

### The Nix flake

Before diving into the pros and cons of Nix, we need to address the engine under the hood: **The Nix Flake**. 

In the traditional Nix world, things were a bit loose.  
You had "channels", but "channels" that could change behind your back... and we don't want to get back to mutable systems.  
Nix flakes fixed this by introducing a strictly defined structure. 

Every Flake has a `flake.nix` (the blueprint) and a `flake.lock` (the time capsule). 

The lockfile is actually the secret sauce.  
It records the exact git commit of every dependency. When I build my system, I’m not just asking for "the latest version of the Odin programming language", but for the exact version of Odin that existed at a specific moment in history.

So, when you run `darwin-rebuild switch --flake .` to update your system (depending on configuration changes), Nix will look first at the lockfile.
If the lockfile says you are using `nixpkgs` from October 12th at 10:45 PM, that is exactly what it fetches. 
Nix doesn't matter if you are on a new MacBook or an old iMac. If the lockfile is the same, then the resulting environment will be identical to your previous setup.

This turns system administration into a pure function: **the same inputs always produce the same result**.

## The "get and forget" workflow

One of the most liberating features is the "ephemeral shell" provided by Nix.  
Imagine you need to test a script against `Python 3.15`, but you don't want to install a pre-release version globally and risk breaking your system tools...

With Nix, you run:

```sh
nix shell nixpkgs#python314
```

Suddenly, `python --version` returns 3.14.
You run your tests, you exit the shell, **and it’s gone**. 

No leftovers, no site-packages conflicts, no trace.
It is the ultimate "get and forget" utility, without messing up your system.

However be careful as **"ephemeral" does not mean "sandboxed" or "isolated"** in this context! 
A common misconception is that `nix shell` is like a Docker container or a VM, but it is not as `nix shell` only isolated the environment variables, and is still running directly on the darwin kernel.

## The learning ~curve~ cliff

Is this configuration exempt from defaults? Absolutely not.  
In fact, its biggest "default" is one of the most significant pain points in modern software: it arrives wrapped in complicated concepts and dense foundations.

The learning curve for Nix is actually not a curve, but a vertical cliff face.

Unlike Homebrew, where you just type a command and forget it, Nix requires you to learn a [domain-specific language called... `Nix` (I know, it is confusing)](https://nix.dev/tutorials/nix-language.html). You have to understand how symbolic links work on macOS, how the Nix daemon interacts with Apple's read-only system volume, and why your applications don't magically appear in Spotlight.

For instance, getting GUI apps to show up in the Applications folder requires "trampoline" binaries or custom activation scripts using `mkalias`.  
After a few minutes debugging and scrolling forums it works great, but many users will simply stop there and walking away with the mistaken impression that `nix-darwin` is an immature project, rather than a deeply powerful one.

The documentation is great, but maybe not well arranged.  
As an example, this is the manual to write your own `flake.nix`: [https://nix-darwin.github.io/nix-darwin/manual/](https://nix-darwin.github.io/nix-darwin/manual/).
You can be lost very easily in the manual if you do not know what you are looking for...

## The hybrid reality

Despite my love for Nix, it isn't perfect for everything. 
macOS GUI applications like Thunderbird, Firefox, or CrossOver, often fight against the read-only nature of the Nix store.  
Those software **expect** to self-update and live in `/Applications` on your system.

I found the sane path to be an hybrid approach: I use `nix-darwin` to manage my critical state (compilers, LSPs, shell aliases), and use the homebrew module inside my Nix config to handle the big GUI pieces. It looks like this:

```Nix
// file: brew.nix
homebrew = {
  enable = true;
  casks = [ 
    "firefox" 
    "thunderbird" 
    "crossover" 
  ];
};
```

That's it! 
And using this, I am sure Nix will use `brew` to install those software on my system, as I did manually with my `brew install` scripts.

This gives me the best of both worlds: Homebrew handles the macOS-specific integration, but Nix still "records" that they should exist in my declarative blueprint.

## How to start

If you want to move away from "hope-based" configuration:

1. install the [Determinate Nix Installer](https://docs.determinate.systems/): it handles the messy macOS multi-user setup for you.

2. initialize a Flake and [`home-manager`](https://nix-community.github.io/home-manager/): Create a `flake.nix` and a `home.nix` for your home-manager instance.

3. think **"modularity"**, like keeping your system settings in `system.nix`, your dev tools in `dev.nix`, and your GUI apps in `brew.nix`.

4. version **everything** and commit your `flake.lock`! It is the source of truth for your system.

If you are curious I’ve documented my current setup, including my security hardening (firewall, stealth mode, etc.) here: [k0pernicus/dotfiles/nix-darwin-config](https://github.com/k0pernicus/dotfiles/tree/master/config/nix-darwin-config).

Keep in mind this configuration is personal, and might not be interesting (and certainly not useful) for everyone.

A few links that helped me to make a first setup version for my mac:

* [https://nixcademy.com/posts/nix-on-macos/](https://nixcademy.com/posts/nix-on-macos/)
* [https://dreamsofcode.io/blog/nix-darwin-my-favorite-package-manager-for-macos](https://dreamsofcode.io/blog/nix-darwin-my-favorite-package-manager-for-macos)
* [https://github.com/torgeir/nix-darwin](https://github.com/torgeir/nix-darwin)

It’s more work upfront, certainly. 
But there is a profound peace of mind in knowing that your system is defined by code, not by a history of forgotten terminal commands.
