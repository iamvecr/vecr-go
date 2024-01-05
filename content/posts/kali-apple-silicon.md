+++
categories = ["Import 2023-10-17 19:33"]
date = 2022-09-26T05:30:45Z
description = ""
draft = false
slug = "kali-apple-silicon"
tags = ["Import 2023-10-17 19:33"]
title = "Kali on Apple Silicon? Not as straightforward as you might think"

+++


Welcome to [Operation Hackerino](https://youtu.be/g6tuepmUmJg?t=10), the objective of the mission is **critical**.

> Setup my laptop as a learning environment for Hack the Box

Easy, right? RIGHT!? As *most* things in life, no.
But first things first, let's grab a [hoodie](https://stockx.com/en-gb/anti-social-social-club-x-hello-kitty-hoodie-black), we can't be learning about hacking if we don't have the uniform on.

### Some background
I want to get myself familiarized with some of the tools and the techniques industry professionals use in the offensive cybersecurity space. I heard about, ⁣
Hack the box, one place where to learn them in a gamified way. There is no course you follow along, but it's a hands-on experience where you complete challenges and level up.
So far the challenges have been laboratories, machines, that you to hack into.

### Current setup
My main computer is 2020 M1 MacBook Air, great performance, long battery life, and it's *fanless*, I love it.

### But! There is a problem
Every professional needs tools, but I thought I could get away by just using the built-in stuff available on macOS, spoiler alert, you can, but you don't want to do it.
Turns out, macOS lacks some command and some programs. For example, a command you give for granted since the dawn of time is telnet, which hasn't been shipped with macOS in a while.[1]

No biggie I thought, thanks to the power of Homebrew, think of it as an App Store but for your terminal, you can find the program you need and thanks to some clever coding and awesome people, you can install it in one simple command without the need to mess around .dmg or installing wizards.
With the power of brew I can install almost everything I need, some tools need to be complied from source and if you start down this path you end up spending more time compiling tools and learning about gcc than progressing with the challenges.

Solution(s)
A solution is to install a Linux distribution, which has every tool installed, alongside macOS in a dual boot environment. But, it might not be worthwhile dealing with the bugs and issues which might occur by running it on Apple Silicon.
I opted for a Virtual Machine and using UTM, a QEMU based virtualization software.

How to

Grab a copy of your favourite Penetration Testing distribution which has native support for ARM based systems if you have an Apple Silicon computer.
I choose Kali, mainly because they have ARM64 builds easy to download and run.

The first problem I had was right off the gate, the graphical installer wouldn't load, but I did found a workaround on GitHub, basically setting the VM to "Console Only" during the installation.


The End


...

But I don't want to work like this! I only want the terminal :(
I want to use SSH with the VM, in order to do that I first check if the server is running. Mine wasn't so, systemctl here, systemctl there, and I started the process and made it auto start during boot.

The networks problem
I want to be able to use the machine on different LANs and I want an easy way to SSH into the VM, so I opted for a loopback interface (<sidenote> is it technically a loopback interface if I use it to connect my host OS to SSH into the guest OS? </sidenote>).
I want to have two virtual interfaces on the virtual machine, one for LANs and the other 'internal' to the laptop, used for communicating between macOS and Kali.
Other virtualization softwares like VMware something PRO have such function, but not UTM.

I turned out to GitHub once again to find other lost souls having the same issue, there I found out, that fortunately QEMU, the underlaying virtualization software supports adding more interfaces, followed an example posted by a user and magically everything worked.
As I understand it, the flag forwards the selected port from the host, e.g. 2504 to the guest's 22 port.

By having two virtual network cards, I could use one to connect to and use the other as an internal network interface only.
To solve another random issue, I also had to change on Kali the 'Cloned MAC address' setting for the interface connected to the host device to 'Preserve'.
My guess is that Kali, would change mac address get a new IP and something would break on Qemu, but it's pure speculation.

Finishing touches
Hack the box requires the user to connect to it using a VPN, to make my life easier I setted up Kali to auto connect to the VPN.

I disabled the desktop environment from autostarting as well in order to keep the VM more slim during runtime.
In order to have VPN access to a browser on my host as well, I create a proxy server thanks to the ssh connection with a simple flag, -D here's the man page about it, cool stuff.  Here's it in action:
```bash
ssh -p 2504 -D 8080 mango@127.0.0.1
```
THE END
I spent a whole Saturday afternoon debugging and learning how to set up everything the way I wanted, I learned a lot and this was not part of Hack the Box itself, but thanks to it, I got to explore new topics, and I'm excited to see what it has to offer.

Mistakes? Typos?
Leave a comment, send a mail or @me on the tweet book. I'm always open to constructive feedback, thank you.