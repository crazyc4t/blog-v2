---
title: "Pickle Rick CTF"
description: Walkthrough of the pickle Rick room on try hack me!
date: 2022-10-27T19:18:39-05:00
image: images/picklerick.gif
license: GPLv3
comments: true
draft: false
toc: true
categories: ["Hacking"]
tags: ["Try Hack Me"]
author: "Said Neder"
---

Hello world! I'm back again with a new blogpost, related to hacking! am preparing myself for the eJPT so I'm taking tons of machines while taking the learning paths from Try Hack Me, specifically the "Pre-Security"

This Rick and Morty themed challenge requires you to exploit a webserver to find 3 ingredients that will help rick make his potion to transform himself back into human from a pickle!

[Room link](https://tryhackme.com/room/picklerick)

**Difficulty: easy**

# Enumeration

We need to hack this website to get information over the 3 ingredients that Rick needs in order to become human again, so let's run nmap, nikto and gobuster while we inspect the source code of the webpage.

## Nmap

```
sudo nmap -p- -Ss -Pn -T5 --open 10.10.67.45 -oG allPorts
```

![nmap](/images/pknmap.png)

Giving the nmap scan we know that two ports are open:

- SSH (22)
- HTTP (80)

And we have that stored on the file named "allPorts"

## Gobuster

```
gobuster -t 30 -u 10.10.67.45 -w common.txt
```

![gobuster](/images/pkbuster.png)

We need to check always the status code of the HTTP request, being 301 **forbidden** but **200** is **OK** so that robots.txt looks good, let's check it out:
`Wubbalubbadubdub`

That is what is was displayed, meaning that word should be used somewhere...

## Nikto

```
nikto -h 10.10.67.45
```

Nikto is a tool that scans the web for us and let's us know vulnerabilities associated with it and important directories we should know about, the important output of the command is a `login.php` route that will come handy later.

## Inspecting the source code

![source](/images/pksource.png)

That's perfect, now we have the username: `R1ckRul3s`

## Gaining access

Now let's go to that `login.php` directory, and let's try the credentials we have gathered:

- Username: **R1ckRul3s**
- Password: **Wubbalubbadubdub**

And we have access!

![companel](/images/pkacc0.png)
![picklerick](/images/pkreal.png)

The other tabs are "blocked" for the real rick, this is the command panel, where we can execute commands, let's try the basics:

- `pwd`
- `whoami`
- `ls -la`

After knowing a bit about were we are, we can now different directories:
![ls](/images/pkls.png)

Being `Sup3rS3cretPickl3Ingred.txt` path, the first ingredient that is a mr. meeseek hair (Look at me!) and the clue is to look around the filesystem and we can do so by executing differents commands at the same time, for example: `cd /; ls -la; whoami`

If we go to `/home/rick` we can see there is a `second ingredients` but is not a directory, instead is a file, so we can just cat it out:
![cat](/images/pkcat.png)

But the command is blocked, for us the future PICKLEEE RICCCCCKKKKK so we need to find another way around, that is using less: `cd /home/rick; less "second ingredients"`

![less](/images/pkless.png)
Being this the answer of the second ingredient.

Time for looking to the third ingredient, by instict we should look first for the `/root` folder, but we don't have permissions so if we run `sudo -l` we can know the privilages we have as the `www-data` user.

![permission](/images/pkperm.png)

This means we can run the `sudo` command without any password! We can't use the command `cd` with sudo beforehand, so let's use `ls` instead.

![lssudo](/images/pklssudo.png)

So we can exploit this vulnerability by using less with sudo: `sudo less /root/3rd.txt`, giving the answer:
![flag](/images/pkflag.png)

And with that we helped rick become human again!

This was a really fun machine easy to get going and learn enumeration for a website, Try Hack Me machines are really fun to get started, as I am doing right now, so expect tons of write ups!
