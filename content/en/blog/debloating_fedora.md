---
title: "What to do after installing Fedora KDE"
date: 2022-03-01T20:59:45-05:00
draft: false
author: "Said Neder"
tags: ["FOSS"]
toc: True
categories: "Linux"
image: "images/fedora.png"
license: GPLv3
---

Welcome again to my blog, it has been a bit of a long time isn't it? But am back once again! And more active than ever so expect so more blogposts coming your way!

## Why am running fedora
As some of you know, I was using [Arch Linux](https://archlinux.org/) with a window manager or "WM" for simplicity, (If you want to see my dotfiles, [here's the repo](https://github.com/crazyc4t/dotfiles)) and It was great! But am on a Laptop, and I needed something more easier to setup since I didn't had the time and in some situations where I needed to take my laptop with me I needed to install some packages and read the wiki, etc for order to make a specific printer work, for example, so it wasn't practical and I just needed something that just worked, bleeding-edge, and a distro that had good documentation and promotes new solutions, and there is where [Fedora](https://getfedora.org/) takes place, since I want to learn how to administrate Linux Servers based on [Red Hat](https://redhat.com) being the company that sponsors the fedora project, it was a perfect fit for me! So in that way I can learn more about DNF (The package manager), Selinux, BTRFS, ZRAM and all of this greatest and latest tech work!

I chose [Fedora KDE Spin](https://spins.fedoraproject.org/) and let me tell you it has been really great! I have always been a fan of the KDE desktop and the mix of the two of them is perfect!

## Step 1: Optimizing DNF

This step is to make DNF, the package manager, be as fast as it can and effective as it can too, by editing it's config file, and we are going to append this strings of text in `/etc/dnf/dnf.conf`

The fastest mirror option is to make sure that DNF checks that it's updating from the fastest repositories, and not the most near ones to you, since am from Ecuador, some of the Ecuadorian repos aren't as fast as the USA ones.

Max parallel downloads is the amount of files is downloading at the same time, so this setting can depend on your internet connection, if have a great internet connection you can sum up the number a bit more.

defaultyes is when DNF asks you about your decision (Y/N) if you press enter the default will be yes.

Keep cache is for the ones that have bad internet connection and if the update or download crashes it has the saved cache of the downloaded packages so you can continue in another time.

```bash
fastestmirror=True
max_parallel_downloads=5
defaultyes=True
keepcache=True
```

Then we will run `sudo dnf up` to update the whole system!

Thanks to [TechHut](https://www.youtube.com/channel/UCjSEJkpGbcZhvo0lr-44X_w) for this step since I read it from him.

## Step 2: Debloating

But KDE, it can be bloated depending of your needs so the first thing I needed to do is debloat it, consisting of removing packages that you are never going to use:

```bash
sudo dnf rm plasma-discover kwrite kontact akregator dragon elisa-player kaddressbook kcharselect kmag kpat kmahjongg kfind kmail kmouth kmines kolourpaint kmousetool konversation korganizer krfb krdc kruler kwrite
```

**Please note, this is depending what you use, I don't use any of this software, if you do don't remove it, read the packages carefully, although the removal of these package will not result in the breakage of the system.**

## Step 3: Setting up rpm-fusion

The repository "rpm-fusion" is what enables us to install Non-free software, like discord, and spotify:

```bash
sudo dnf install \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

 sudo dnf install \
  https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```
After that just `sudo dnf up` and that is it!

## Step 4: Loading up some software!

Install some software! This is the software I install most of the time, you can take a look if you want:

**This is just my selection of software, the tools I use the most of the time, you should install the software that you use, if not sure just skip this step.**

```bash
sudo dnf in kate alacritty neovim git fish vlc gimp kdenlive texlive-scheme-medium lpf-spotify-client discord keepassxc qbittorrent stacer doas
```

And if you use Brave browser like me, you can use this code to easily install it:

```bash
sudo dnf install dnf-plugins-core

sudo dnf config-manager --add-repo https://brave-browser-rpm-release.s3.brave.com/x86_64/

sudo rpm --import https://brave-browser-rpm-release.s3.brave.com/brave-core.asc

sudo dnf install brave-browser
```

If you use zoom and wish to install it, you will need to download the .rpm package from the website linked [here](https://www.youtube.com/channel/UCjSEJkpGbcZhvo0lr-44X_w)

### Setting up some media codecs

To make everything work properly when talking about videos and music, we are going to install these media codecs:

```bash
sudo dnf install gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel

sudo dnf install lame\* --exclude=lame-devel

sudo dnf group upgrade --with-optional Multimedia
```

More info [here](https://docs.fedoraproject.org/en-US/quick-docs/assembly_installing-plugins-for-playing-movies-and-music/)

### Cleaning up 

This is as easy as `sudo dnf autoremove` and/or you can use stacer to clean your system as well.

## Outro

This is my quick setup guide of fedora KDE! Probably am going to write a script that automates this process, stay tuned for that, just wanted to say thank you for reading my posts, wishing you all a great start of the month, time to get after it! Love from Ecuador.

Thanks so much for the fedora's subreddit [r/Fedora](https://www.reddit.com/r/Fedora/) helping me to improve my post!
