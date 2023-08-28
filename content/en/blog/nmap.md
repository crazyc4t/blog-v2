---
title: "Deep dive into nmap"
description: In depth look at scanning with Nmap, a powerful network scanning tool.
date: 2022-11-05T13:17:13-05:00
image: images/nmaplogo.jpg
license: GPLv3
comments: true
draft: false
toc: true
categories: ["Hacking", "Tools"]
tags: ["Try Hack Me"]
author: "Said Neder"
---

When it comes to hacking, knowledge is power, the more you know, the more options you have to attack, making critical a proper enumeration before any type of exploitation.

Before attacking our target we need to know what is what we are about to attack, we need to know what type of services or OS is running, and we can accomplish that by making a network map, hence the name of Nmap, specifically doing "port scanning", because these services are listening on a specific "ports" of the network, being ports a network structure your service runs on to establish a connection, the service are always "listening" (waiting for another device that wants to establish a connection) and the user when connecting to the specific port they open a port for receiving information from the other port (for example: HTTPS 443)

![image](/images/nmap.png)

All of this reconnaissance we can achieve using nmap, so keep reading!

## Nmap Arguments

Let's get our hands dirty, open the terminal and install nmap (comes installed by default in kali linux and parrot os) and run:

```bash
nmap -h
```

To know which arguments can be used in nmap, listing some basic and really useful arguments:

- `-sS`: Syn Scan
- `-sU`: UPD Scan
- `-O`: OS detection
- `-sV`: Version of the service running on ports
- `-vvv`: Verbosity 3x
- `-oA, -oN, -oS, -oG`: Gives the output on different formats (all, normal, script kiddie, grepable)
- `-A`: Aggresive (I don't care if they know someone is scanning them, give me all the info!)
- `-T5`: Go fast g
- `-p xx`: Only scan xx port
- `-p-`: Scan all ports

## Types of Scans

We are going to look more in-depth of the types of scanning that are available like:

- TCP connect scans (`-sT`)
- SYN "half-open" scans (`-sS`)
- UDP Scans (`-sU`)

## TCP Connect Scans

This type of scans are made when completing the three-way handshake:

![image1](/images/nmap1.png)

Where it sends a SYN packet to the specified port, if it receives the SYN/ACK, it will reply with ACK, and terminating the handshake, having closed the connection, it will know that the port is open.

**What happens if it's closed?**
When sending a SYN packet, the target replies with RST (reset), that means the port is closed (does not exist)

![image2](/images/nmap2.png)

**If it's behind a firewall?**
It will just drop the connection, it won't reply back at all, and when this happens, nmap declares these types of ports being **filtered**

Example:
`nmap -sT <ip>`

## SYN Scans

These are similar to the TCP scans, but are referred as "half-open" or "stealth", because when receiving the SYN/ACK packet, it will send instead of ACK, RST, closing the connection early and therefore no letting the target re-send packets again, being faster and stealthier because the majority of the times only full TCP connections are logged, and this is not one of those.

Example:
`nmap -sS <ip>`

![nmap3](/images/nmap3.png)

## UDP Scans

UDP is stateless, being that it does not require a established connection to transmit packets, so nmap just send packets to the target, and if the target does not reply back it the port is flagged as **open|filtered** because does not reply it could be filtered because of a firewall between it, and nmap knows that is closed when it sends a ICMP packet back with a message that the port is unreachable, knowing exactly that the port is closed.

Because of the lack of acknowledgement of UDP, UDP scans are terrible slow, so you should only scan the top 20 UDP ports, for example: `nmap -sU --top-ports 20 <ip>`

## NULL, FIN and Xmas

This are stealthier types of scan, so let's briefly go through them:

### NULL

This is when instead of sending a SYN packet, it sends a NULL packet, being essentially none, nothing, and the reply back is a RST/ACK.

Example: `nmap -sN <ip>`

![nmap4](/images/nmap4.png)

### FIN

It is the same as NULL, but instead is a FIN packet (final), that is used to end the connection, so the reply is the same as NULL.

Example: `nmap -sF <ip>`

![nmap5](/images/nmap5.png)

## Xmas

Xmas scans send a malformed TCP packet and expects a RST reply for closed ports.
![nmap6](/images/nmap6.png)

But all of these scans does not expect a reply for knowning open ports, like UDP scans, so they will acknowledge ports closed or open|filtered, but only filtered as well when receiving an ICMP unreachable packet.

The goal here is to bypass firewalls, this is not 100% effective since most modern IDS (Intrusion Detection System) are smart about this.

It is common that MS Windows reply to every NULL, FIN, XMAS scan with a RST.

Example: `nmap -sX <ip>`

## ICMP Network Scanning

This comes in place in a "black box" scenario, where we don't know nothing, so our first step is to create a map of active hosts, we can accomplish doing this with a "ping sweep" where we send a ICMP packet to every host in the subnet of the IP given, and expect they will receive it to know they are active.

Example: `nmap -sn 192.168.0.0/24`

This does not scan ports, however it will send a TCP SYN packet to ports 443 and 80.

## NSE Overview

The Nmap Scritpting Engine is a powerful addition to nmap, being written in lua is really easy to do tons of things, like scanning for vulnerabilities, even to exploiting, these are the most popular and useful categories:

- `safe`: Won't affect the target
- `intrusive`: Not safe: likely to affect the target
- `vuln`: Scan for vulnerabilities
- `exploit`: Attempt to exploit a vulnerability
- `auth`: Attempt to bypass authentication for running services (e.g. Log into an FTP server anonymously)
- `brute`: Attempt to bruteforce credentials for running services
- `discovery`: Attempt to query running services for further information about the network (e.g. query an SNMP server).

[Learn more about NSE here](https://nmap.org/nsedoc/)

All the NSE scripts are located on `/usr/share/nmap/scripts/`

## Outro

Special thanks to the [Try Hack Me nmap room](https://tryhackme.com/room/furthernmap) since thanks to that room I learned more in-depth about nmap.

Thanks for reading! I will upload more blogposts specified on certain pentesting tools like this one!
