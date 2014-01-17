---
layout: post
title: "Setup Acer C720 Chromebook As Development Computer"
date: 2014-01-08 22:27:33 +0800
comments: true
categories: [chromebook, crouton]
author: yongzhy
---

Today is my second day with Acer C720 chromebook. After two days usage, my impression on chromebook is totally impressive. My initial intension was to buy a Macbook Pro 13 retina display. But while waiting for the Apple "Red Friday" discount before Chinese New Year, I was thinking again and again "do I really need to buy a powerful Macbook Pro that cost over S$2000?"

I have a powerful laptop at home now, but because of its bad battery life, everytime I use it, I need to plug in the power adapter. I can only use laptop after the two kids went to sleep, and I want to use it when on the bed.  The requirement to connect to power adapter just keep me away from use that laptop. What I need is a device with very good battery life and meet the requirements below:

* Battery must be last at least 5-7 hours
* Must have keyboard, one of the main task of this device is for me to write blog or code on bed before sleep.
* Can run vim, I use it as my primary code editor
* x86 CPU, no need to be very powerful
* Hard disc space no hard requirement, as long as there are few GB left for me to install some development tools
* Memory at least 4GB

Acer C720 meet all requirement except the 4GB RAM, it's because 4GB version is out of stock on Amazon. And because of Singapore telco Starthub promotion, someone was selling cheap of C720 after they got this as free when sign broadband contract with Starhub. So I just contact one guy in a forum and offer S$300 for this 2GB version C720.

I got it yesterday after work. After dinner, I started the process to install Ubuntu 13.10 using crouton. The process was very smooth. Below are what I have done to setup my chromebook.

* enable developer mode
* install Ubuntu 13.10 using crouton
* install vim, git, build_essential, cmake, golang1.2, llvm3.4
* build the_silver_searcher, YouCompletme for vim
* install ruby and necessary setup for blog system Octopress

After all of those setup, I still have over 7GB space left, that's far better than my expection. My overall experience on this chromebook is excellet. But if there are other choices of chrombook available, I would want a configuration with 4GB RAM, 32GB SSD. Current 2GB RAM is just not enough for serious development.
