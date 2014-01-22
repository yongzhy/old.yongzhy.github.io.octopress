---
layout: post
title: "Turn C720 Into Development Machine: Step By Step Guide"
date: 2014-01-22 22:52:54 +0800
comments: true
author: yongzhy
categories: [chromebook, ubuntu, crouton]
---

From Google Analytic report of my blog,  I saw there are two visitors, one from Germany and the other from US. Both of them viewed my blog post about [setup C720 development environment](http://zhuyong.me/blog/2014/01/08/setup-acer-c720-chromebook-as-development-computer/).  But in that post, I didn't record down step by step how to setup the development environment. Since people are interested in this topic, I think I should write a formal and detail post about this. So, here come this step by step guide of turn your C720 as development machine by install Ubuntu using [crouton](https://github.com/dnschneid/crouton). 

The reason I choose `crouton` over `chrubuntu` is because of the super convenient way to switch between ChromeOS and Ubuntu with `Ctrl + Alt + Shift + Back` keys. There is no need to reboot machine to switch between the OSs. 

#### Step 1: Enable Developer Mode

Enable the developer mode will wipe all your data, so backup any important data before enable developer mode. 

To enable developer mode:

* Press `Esc + Refresh (F3)` keys then press `Power` button
* Press `Ctrl + D`, ignore the warning/error message on screen.
* Press `Ctrl + D` again, you entered the developer mode.

After enable developer mode, evertime you cold start your computer, you need to press `Ctr + D` or just wait for 30 second.

#### Step 2: Install Ubuntu 13.10 Using crouton

`crouton` is a powerful script that will do all the download and installation job for you. Download latest version of `crouton` from [http://goo.gl/fd3zc](http://goo.gl/fd3zc). For details of crouton usage, refer to it's [github repo](https://github.com/dnschneid/crouton). 

I use `crouton` to install Ubuntu 13.10, and I choose `xfce4` as the windows manager. Below are the steps I prerformed:

* Download `crouton`
* Open shell by `Ctrl + Alt + T`, then type `shell`
* Run `sudo sh -e ~/Downloads/crouton -r saucy -t xfce`. This will start the downloading and installation process for Ubuntu 13.10
* Wait untill the installation done.
* Run `sudo startxfce4` to start the `Xfce`
* Anytime you want to switch between ChromeOs and Ubuntu, just use `Ctrl + Alt + Shift + Back` or `Ctrl + Alt + Shift + Forward`
* log out `xfce` will end the `xfce` session

#### Step 3: Install Development Tools

There is no much to talk about this step, it's the same as install development tools in Ubuntu. Just run `sudo apt-get update`, `sudo apt-get install` to install whatever tools you needed. 

For me, I installed `git`, `vim`, `build_essential`, `cmake`, `golang1.2`, `llvm3.4`, `the_silver_searcher` as well as `ruby` and necessory packages for `Octopress`.

If you followed all the steps above, your C720 will become a nice Linux development machine. I haven't tried to run any IDE like `eclipse` or `android studio`, I think my 2GB RAM may not sufficient for RAM hunger tools like that. If you have the 4GB version of C720, I think there is no problem to run those IDEs. 

#### Step 4: Key Map Modification (Optional)

If you are like me, always use `CapLock` as `Ctrl`, you can follow the instruction in my another [post](http://zhuyong.me/blog/2014/01/15/map-search-key-to-control-key-on-acer-c720-chromebook/) to map the search key to ctrl.

**Next**, I will write another post to talk about development on C720 using cloud based development environment. If you are interested, please stay tuned.