---
layout: post
title: "Config Logitech Performance MX Zoom Button In Ubuntu"
date: 2015-11-05 13:42:07 +0800
comments: true
categories: [ubuntu, linux]
author: Zhu Yong
---

The middle button (click wheel) on Logitech Performance MX is very very difficult to press. So I changed the `Zoom` button to middle button on windows system. To do same setting under Ubuntu, follow the instructions below:

* Install necessary software packages

```
   sudo apt-get install xbindkeys xautomation xev
```

* Next, use `xev` to find the button number for `Zoom` button. Just run `xev` from command line, then click `Zoom` button when your cursor is in the small white window. You will see somthing like below:

```
    ButtonPress event, serial 45, synthetic NO, window 0x2400001,
        root 0x24d, subw 0x0, time 3735892633, (75,111), root:(301,192),
        state 0x0, button 2, same_screen YES
    
    ButtonRelease event, serial 45, synthetic NO, window 0x2400001,
        root 0x24d, subw 0x0, time 3735892633, (75,111), root:(301,192),
        state 0x200, button 2, same_screen YES
```

* Create default `xbindkeys` config file with command: 

```
   xbindkeys --defaults > ~/.xbindkeysrc
```

* Open file `~/.xbindkeysrc` to add the following lines at end of the file:

```
    "xte 'mouseclick 2'"
    b:13+Release
```

