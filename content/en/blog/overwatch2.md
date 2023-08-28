---
title: "How to play Overwatch 2 in linux"
description: This guide will help you install all the requirements for playing Overwatch 2 on linux.
date: 2022-10-08T13:07:44-05:00
image: "images/overwatch2.jpeg"
license: GPLv3
comments: true
draft: false
toc: true
categories: "Linux"
tags: ["FOSS"]
author: "Said Neder"
---

Hello guys! First of all, thanks for reading my blog, overwatch 2 has been released and it's time for us to try it!

Playing it on linux is really simple, so feel free to follow along!

I'm using arch linux when following this guide, if you are not running arch linux as your linux distribution, don't worry! Just make sure everything is up-to-date with your system.

## Installing the requirements

First of all we need to install:
- Lutris
- Wine 
- Drivers

Lutris is a free and open source software that let us play windows games in linux by using "wine".

We can install lutris pretty easily, I will leave a guide for all linux distributions [here.](https://lutris.net/downloads)

For drivers and wine [lutris](https://lutris.net) has documentation on the topic, you can read [here](https://github.com/lutris/docs/blob/master/WineDependencies.md) for wine dependencies and [here](https://github.com/lutris/docs/blob/master/InstallingDrivers.md) for drivers.

## Installing Battle Net

This step is very easy, we need to open lutris, click in the "runner" section the little drop-down box besides wine:

![winesection](/images/winesection.png)

Install "overwatch2-caffe" wine runner, since it's the runner we are going to use for installing battle.net:

![wineinstall](/images/wineinstall.png)

Then we can go to [lutris website](https://lutris.net/games/battlenet/) and install battle.net by clicking the "install" button on the webpage, if that doesn't work, you can go to the search bar inside lutris and search for battle.net, after that you can let it install.

Follow the installation wizard of battle.net, **disable the check-box of auto-starting battle.net when the computer starts** and when it prompts you to login, do not login yet, just close the program and let the lutris installation wizard finish.

### Using caffe runner 

When finishing the installation, you need to select battle.net, then press on the up-arrow button besides the "play" button:

![configure](/images/runner.png)

After that press configure, under the "runner" tab, there is the "wine version" that will be "overwatch2-caffe", if it does not show in the selection, please read carefully again on how to install runners under the [Installing Battle net](#installing-battle-net)
![runnertab](/images/runnertab.png)

After selecting that runner, run battle.net and install overwatch and happy gaming!

## Compiling shaders...

If you feel that overwatch is slow and noticed the sentence "compiling shaders..." in the under left corner, it's because wine is compiling shaders as it says, meaning that is loading graphics for the game to be smooth, so if you just wait around 2-3 minutes it will be gone and the game will be smooth.

**Remember** the game compatibility in linux is on early stage, so is normal for the game to be sometimes a bit buggy or does it no look that good, but the situation will improve rapidly over time, but right now enjoy!

## Troubleshooting

- Battle.net is not starting?!
  - Check you are using the caffe runner, if it keeps happening, disable the fsync setting under the runner tab.
