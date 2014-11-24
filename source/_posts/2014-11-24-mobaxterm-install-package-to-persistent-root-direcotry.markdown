---
layout: post
title: "MobaXterm Install Package To Persistent Root Direcotry"
date: 2014-11-24 13:51:51 +0800
comments: true
categories: MobaXterm
author: Zhu Yong
---

[MobaXterm](http://mobaxterm.mobatek.net) is a very good SSH and X-Windows client. I have being use the free portable personal version for quite a while. Its built-in X-Server is very handy and convenient for people like me to run remote GUI applications occasionally. From version 7.2, they added the `apt-get` like package manager: `MobApt Package Manager`, and added the option to set `persistent root` directory, so user can install their own cygwin package to this persistent root directory. This should be the most desirable feature for all personal edition users, with this feature, package upgrading and installation is within one's fingertips. 

But at Version 7.2, when I tried use this `MobApt` to install `git`, after installation done, `git` command not recognized when I typed in shell. After checked the installation process, I found a lot of link to `git.exe` were not created successfully, so I manually created those links, and `git` command works, but this is a painful process. 

Today, when I setup `MobaXterm` Vesion 7.3 on my laptop, I suddenly realized that maybe I should run `MobaXterm` as Administrator for it to create those links successfully. And when I did that, result shown that I was correct, everything installed nicely. Next, I went back to my desktop PC, the process didn't succeed, I think I need a fresh installation, so I deleted the whole persistent root folder and persistent home folder on my desktop PC and went through the whole process again. Bingo, `git` works like a charm. Why I didn't realized this when I use Version 7.2??

When I remove the persistent root folder in Windows 7, I say message that some file / folder can't be removed because of `Permission Denined`. After few seconds Google, solution is simple: 

    takeown /r /f DRIVE:\PATH
    icacls DRIVE:\PATH /grant USERNAME:F /T

To conclude, next time, I must run `MobaXterm` as Administrator to install any new package or upgrade existing packages.
