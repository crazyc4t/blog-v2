{
  "title": "How to free yourself by installing linux",
  "date": "2021-11-21T01:40:22-05:00",
  "draft": false,
  "author": "Said Neder",
  "tags": ["FOSS"],
  "toc": true,
  "categories": "Linux",
  "image": "/images/lmdeskdemo.png"
}

## Intro

This is a special blogpost since this is the guide to switch from windows to linux for new users, and the way to free them of propietary software and enjoy their freedom with free software, the software you should run!

We are going to divide this guide in steps:

## Intro
For the FAQ's I made a [blogpost](https://saidneder.tech/blog/penguin) that covers this questions:

- What is Linux?

- What is GNU?

- What is GNU/Linux?

- What are distros?

- Why should I care?

- Should I switch?

It does cover this questions in a bit of technical way tho, but check it out if you want to learn more!

The "difficult" part of linux is unlearning windows, since here in linux we are different in this aspects:

- Privacy and security

- Updating and installing software

- Philosophy

But don't worry! Am here to explain to you every step of the way and to enjoy once and for all your free computer, free as in freedom.

Am going to cover the topics above broadly...

# Privacy and security
The only privacy setting you need to turn on/off is location for the weather, meaning that no sending "anonymous" data to big companies or similar cases, and linux being based as a unix-like operating system it works with multi-users, being always two users:
- Root

	- being root is being god in other words, you can do anything and everything, so this is the user we are not going to use.

- User (you)
	
	- This is where you come in place, all of your configurations and files are stored on your "home" folder, and if something happens it will only run as your user, which doesn't have any type of permission at the OS level, so no worries, and viruses are not developed for linux since is a OS that the minority uses, now when we need for root perrmissions temporarly, we ask root if we can borrow them for a bit using a command named "sudo" (super user do) so we should never log in as root only if you know what you are doing.

# Installing software
This and the security, for me are the selling points for linux, installing software on linux is really easy, no more of going to a website to download a .exe just to figure out it was a virus...
Installing software on linux is straight-forward, the software you want is on a repository or known as "repos" (server) and what you do is to install directly from that official server, being this simplified, you just go to an app store that is a pre-installed program and just download your software and you are ready to go! you will get updates in the same app store, so not to worry about it.

# Philosophy
I already tackled a bit of this topic on another [blogpost](https://saidneder.tech/blog/penguin) but talking this broadly, is about the philosophy of community, since linux is a community driven by free and open-source software, that users control it and doesn't control them, about being helpful, asking questions when you don't understand something, searching your question on the web, being curious, and to support and say free software, since this is the software we need to fight for our freedom.

# Steps to follow:
0.  Compatibilty

1.  Flashing

2.  Installing

3.  Freedom!


## Step 0: Compatibility.

This is a easy step, this step is to check the software we use if there is a linux version or alternative, since Adobe software or Microsoft Office is not available for linux, but there are alternatives, for example:

- Office Suite = [Libre office](https://www.libreoffice.org/)

- Photo editor =  [GIMP](https://www.gimp.org/)

- Vector Graphics = [Inkscape](https://inkscape.org/)

- Video Editor = [Kdenlive](https://kdenlive.org/en/)

- Drawing software = [Krita](https://krita.org/en/)

- DJ software = [Mixxx](https://mixxx.org/)

- 3D Modeling = [Blender](https://www.blender.org/)

- CAD Software = [FreeCAD](https://www.freecadweb.org/)

Browsers, spotify, discord, or others are all on linux, so no worries by that side!

And gaming is great too! Steam being on all repos and the majority of games run with the [proton](https://www.protondb.com/) compatibility layer.

And in this step we need to back up all of our files because once we install linux our files are going to be **DELETED** so please take a lot of caution, back up all of your important files.

## Step 1: Flashing
Flashing, what is that? let me explain...
To install Linux the only requirement we need to have besides our computer is a flash drive (or known as a pen drive), 4gb of space is perfect to (as the word says) flash linux, but before getting to that step we need to **choose our distro.**

### Choosing our distro
A distro is a operating system based on GNU/Linux, and there are a **ton** of distros, but we are going to choose just one, and a good one of course.

Since this is a beginner guide, we are going to choose beginner friendly distros, and the three options that I recommend are:

- [Linux Mint](https://linuxmint.com/)

- [Elementary OS](https://elementary.io/)

- [Pop!_OS](https://pop.system76.com/)

This three distros are **free as in freedom but free as in free beer too!** , but what are the differences are which one should you choose? well, depends who you are: 

- Pop!_OS 

OS created by [system 76](https://system76.com/) an American computer company that desings their computers with freedom and linux in mind, being this distro good for tech-savy people, gamers, coders, or **curious** people, specially if you have a nvidia card, just go with this option.

- Linux Mint

OS made for users that wants their computer to **just werk**, equiped with software, desinged with functionality in mind with a user interface easy to use, really good with old and low-end hardware, similar to windows 7, fast, secure and it just works.

- Elementary OS

OS made for **creative** users, with a beutiful desktop enviroment similar to Mac OS, being feature-rich like multitasking view, picture in picture, and a lot more, being a clean and polished distro with a beutiful desktop.

When you have made your mind, time to download the .iso! that is the OS itself, in a file, so go to the webpage of the distro you have chosen (Up in the blog there are the links, below choosing our distro) and download the file!

Is straight-foward but a note:

- 	For mint users: when you go to the download page there are three options: xfce, mate, and cinnamon, now:
	-  Xfce = For really old or low-end hardware, user interface isn't the prettiest. 
	
	-  Mate =  For old hardware, stability and traditional feeling, not a lot of features.
	
	-  Cinnamon = Active development, modern, good look and feel, recommended for 2 GB of ram and up. (go with this one)

-  For elementary OS users: At the front of the screen it says "purchase elementary OS", and above it says "pay what you can" I highly recommend paying for the OS, at least $5 bucks, but if that's not the case for you, you can insert a digit in the "custom" box, and insert 0 and download it for free!

-  For Pop_OS! users: There are two versions,the nVidia one and the intel/amd one, download the one according to your hardware, this is pretty straight-foward too, if you don't know what nVidia is, then just download the intel/amd one.

### Flashing our distro
Download etcher on their [website](https://www.balena.io/etcher/) and install it, then time to plug your flash drive but **WARNING** everything in the flash drive **WILL BE DELETED**, so back up your files, now just follow the steps in screen, that is the first step to select the .iso that you downloaded, the second to select your flash drive and the third to flash, and should look like so:

![etcherdemo.png](/images/etcherdemo.png)

After this, we are **READY!!!!** this should be exciting!

## Step 2: Installing

Before booting into linux, we need to make some changes in bios, now this could look scary but it really isn't! (make sure your usb is connected)

Now follow this steps and you will be fine:

- Reboot your computer

- Before your laptop starts up, press the ESC or DELETE key (top right and top left) to enter bios

- Now on BIOS, depending what computer you have you can use your mouse or just your keyboard, now navigate too BOOT section with the arrow keys, and deactive SECURE BOOT (blame microsoft) and then you will have a boot order, change the order being the flash drive the first priority.

- Now go the SAVE section and save settings and reboot.

- Now you should boot into linux!

If you made this successfully, congrats! you have made the hardest thing in this whole step!

Now you are on the desktop of your chosen linux distro! (except elementary OS, follow the installer and choose "demo mode")

![linuxmintdeskdemo.png](/images/linuxmintdeskdemo.png)

Being for me the linux mint desktop!

This is the perfect moment to check everything out, remember it won't be as fast since is running from a simple USB, but you can get an idea of what you are going to get, so check the desktop, your wifi, bluetooth, mouse, keyboard, everything, once you are happy about that everything works (and if it does not, search your error on the web, since this type of errors have easy fix) now is time to continue with the installer, just follow the on screen instructions and you will be fine!

Note: If you have the option to install multimedia codecs, do it! is for managing audio and video files.

Now here is a side-note, there's something called "dual booting" that is having linux alongside windows, and when you boot your computer you will decide what operating system you want to use, and this could be great as it could be a little bit of a headache, I have dual booted before and my experience has been okay-ish, but other users have reported that when Windows updates have deleted their linux partition, or have deleted their boot entry (that has happen to me before and I couldn't recover linux, but it was my fault in certain way) so you can have a dual boot, or just wipe out windows and have only linux (recommended way) but finally is your computer and you have the last word, if you decide to dual boot, your installer will give you the option and you will need to decide how much space you want to give each OS, so take that in mind since windows is bloated and heavy, so for me a clean install is the best option but is your computer after all.

Now, along the installer you will have the option to encrypt your drive, **please do it** it will increase your security being the case that someone has access to your computer fisically they can't do anything about it since the information is encrypted and your data protected, is a huge plus!

![linuxmintinstalldemo.png](/images/linuxmintinstalldemo.png)

(this is the case for linux mint, to encrypt your home folder)

After all, we are golden! Welcome to Linux!

![lmdeskdemo.png](/images/lmdeskdemo.png)

## Step 3: Freedom

Now this is the final step, enjoy your freedom! now you are free to do whatever you want, so go explore! code a bit, learn the command line, install gimp and edit some pics, go crazy!
But there's some steps I would recommend to do before you go crazy!
- Update the system 
  - (you can do so by inserting this command into the terminal `sudo apt update && sudo apt upgrade`)
- 	Install media codecs
    - (you can do that by executing this command in the terminal: `sudo apt install ubuntu-restricted-extras`)

Those steps is to get everything updated and ready to rock! now go crazy!


## Outro
Hoped that you liked this blogpost, there's a lot of heart on this specific post, so if you like my content you can share it with your friends or buy me a coffee down below!

This are resources that can help you!
 - For the whole process you can take this [video](https://youtu.be/_Ua-d9OeUOg) to guide you made by Anthony Young from Linus tech tips!

- Pop_OS!
	- [5 Things to do after installing Pop!_OS by TechHut](https://youtu.be/FgJ1k476ds0)
	- [Pop!_OS FULL beginners guide by Learn Linux TV](https://youtu.be/4mySqL4bCSw)

- Linux Mint
	- [From noob to power user with Linux Mint Cinnamon! By Derek Taylor](https://youtu.be/TKX29fJ8U2Y)

- Elementary OS
	- [Elementary OS Review By Mental Outlaw (Ma boi kenny)](https://youtu.be/zHvQZqN1Kjw) (This video is not that updated but is good!)

And that's it for me! Am glad that I can help you start your path into linux, so enjoy it, make it yours and go crazy! You can contact me in any way if you have a question, my discord code is on the About me [page](https://saidneder.tech/about/)!



