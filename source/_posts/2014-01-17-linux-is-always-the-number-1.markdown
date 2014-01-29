---
layout: post
title: "Linux Is Always The Number 1?"
date: 2014-01-17 13:59:00 +0800
comments: true
author: Zhu Yong
categories: [linux, gcc, c]
---

Yesterday I read an article [C language and the linux macro - proof that linux is always Number 1](http://arjunsreedharan.org/post/71403510912/c-language-and-the-linux-macro-proof-that-linux-is). It shows a simple C code which just define variable `linux` and assign with a value, but `gcc` produce error when compile it.

Code of the simple program:

    #include <stdio.h>
    
    int main() {
        int linux = 701;
	    printf("%d", linux);
	    return 0;
    }
    
Error message from `gcc` :

    code.c: In function ‘main’:
    code.c:4:6: error: expected identifier or ‘(’ before numeric constant
    int linux = 701;
        ^
If preprocess the program using `gcc -E code.c`, the output programe code is:

    int main() {
        int 1 = 701;
        printf("%d\n", 1);
        return 0;
    }
    
It's very obvious that `linux` is `#define` somewhere with value `1`. But the code only included `stdio.h` file and there is no such define in this header file. After search Google, I have found the explanantion from stackoverflow. 

Word such as `unix` or `vax` was defined in the pre-ANSI days to detect the system at compile time. To make the compile process pass, just pass parameter `-std=c89` or `-std=c99` to `gcc` will do.

#### Related Questions From Stackoverflow:

* [Stackoverflow Question 1](http://stackoverflow.com/questions/19210935/why-does-the-c-preprocessor-interpret-the-word-linux-as-the-constant-1)
* [Stackoverflow Question 2](http://stackoverflow.com/questions/3770322/is-unix-restricted-keyword-in-c)
