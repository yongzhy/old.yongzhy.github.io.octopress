---
layout: post
title: "ARM RVCT tips: armcc &amp;&amp; fromelf"
date: 2014-12-12 15:54:19 +0800
comments: true
categories: [c, armcc, fromelf]
authoer: Zhu Yong
---

For embedded system C projects that share same code base, one common practice is to use various feature flags to encapsulate related code. This results difficulty in code reading, especially when you have huge amounts of feature flags, especially when flags' value depend on value of other flags. This is what I am dealing with in current project. And I found `armcc` and `fromelf` have some useful feature to help developer in such situation. 

First, `armcc`, the compiler from ARM RVCT. Command line options are `--debug --debug_macros -E`, this will generate raw C file with all preprocessors been parsed, you can see how is a macro been defined. 

* `--debug` is to enable generate debug information
* `--debug_macros` is generate debug entry for macros. You can see the expanded macro in the generated C source code. [doc here](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.dui0491h/CIHHDEAA.html)
* `-E` is to tell compiler not compile the C file, just excute the preprocess step. [doc here](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.dui0491h/CIHIDBBG.html) 

Second, `fromelf`, imaging you have a complicated data structure defined, of which, some elements depends on some feature flags, to find out the difference between two different configurations, instead of load into debugger and compare, you can just use `fromelf` to dump the structure and use a diff tool to compare. 

The command line is `fromelf --fieldoffsets --select=the_structure.* --output=the_structure.txt final.axf `

* `--fieldoffsets` produce offset for each structure elements. [doc here](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.dui0477i/BABJBGBH.html)
* `--select=the_structure.*` to select the data structure to output
* `--output` to specify output file contain the listing


