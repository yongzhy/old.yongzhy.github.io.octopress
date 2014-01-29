---
layout: post
title: "Map Search Key To Control Key On Acer C720 Chromebook"
date: 2014-01-15 20:00:46 +0800
comments: true
author:  Zhu Yong
categories: chromebook
---

I am a vim user, and for all my home and office computers, I map the CapLock key to Ctrl key. So after installed Ubuntu 13.10 using crouton on my new Acer C720 Chromebook, I was thinking to map the search key to control key. I did some search online as well as some experiments and finally found the solution.

I choosed xfce4 as my window manager because of the rather low 2GB RAM. By default, the search key is mapped to Super_L after install Ubuntu using crouton. If you want to know the keycode for each key on the keyboard, you can use `xev` to check.

I use `xmodmap` to do the key mapping.

First, create file `.Xmodmap` at your `$HOME` directory with content below

    clear control
    clear mod4
    keycode 133 = Super_L
    add control  = Control_L Control_R Super_L
    add mod4 = Hyper_L Super_R

Next, need to make sure `xmodmap` load `~/.Xmodmap` file when you start you linux using `sudo startxfce4` in chromebook terminal. Create file `~/.xinitrc` with content below

    if [-s ~/.Xmodmap]; then
        xmodmap ~/.Xmodmap
    fi

Last step is to logout your xfce environment and restart it from chromebook terminal. 

#### References Websites

* [http://cs.gmu.edu/~sean/stuff/n800/keyboard/old.html](http://cs.gmu.edu/~sean/stuff/n800/keyboard/old.html)
* [https://wiki.archlinux.org/index.php/Xmodmap](https://wiki.archlinux.org/index.php/Xmodmap)
* [https://github.com/dnschneid/crouton/wiki/Keyboard](https://github.com/dnschneid/crouton/wiki/Keyboard)

