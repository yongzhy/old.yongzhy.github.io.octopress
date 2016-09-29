---
author: Zhu Yong
categories: 
- coding
comments: true
date: 2014-12-31T15:12:54Z
title: 'T32Hook: Utility To Auto Hook Trace32'
url: /blog/2014/12/31/t32hook-utility-to-auto-hook-trace32/
---

Today is last day of 2014. I will use this 2014 last blog to write about the utility I recently developed. 

On my latest project, firmware code failed during test with power cycle on target. And the Lauterbach emulator can't detect the `power on` event on this target, so we can't use the `power on` event to automate initial script running. The failure only happens after power cycle, so I must let Trace32 run the setup script to enable the trace in order to capture the code running history.   

Because previously I already did a utility to automate failure replay using Trace32 simulation, I know Trace32 has remote API to control the script running. So I come out with the idea to detect target power cycle from the message on serial port (target firmware will print some message on serial port), upon power cycle event happen, use remote API to command Trace32 to execute the setup script. 

QT already has the `The Simple Terminal` example, so my development leverage on this example code and added Trace32 remote API code. 

This is the image of the main window for the final working product.

{{< figure src="/images/blogimg/2014-12-31-t32hook/t32hook-001.png" >}}

This the serial setting dialog, I didn't change this part from the example code.

{{< figure src="/images/blogimg/2014-12-31-t32hook/t32hook-002.png" >}}

This is the Trace32 remote setting dialog. Added options to specify the remote host, port, packet size, the setup script to run as well as text line to trigger setup script re-run. 

{{< figure src="/images/blogimg/2014-12-31-t32hook/t32hook-003.png" >}}

The whole development take me few night time at home. My initial working version with everything hard coded, because we must solve the firmware issue ASAP. I use my spare time to make it configurable, so my colleagues with same need can use it as well. 

This is not my first utility written using QT, when I need to write GUI utility, I will always turn to QT, sometime use PyQT, sometime use the native C++ version. The latest version of QT already have support for Android and iOS, I may test water with the Android version some time later.   
