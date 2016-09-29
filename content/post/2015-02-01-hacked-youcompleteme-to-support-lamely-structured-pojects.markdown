---
author: Zhu Yong
categories:
- vim
- clang
comments: true
date: 2015-02-01T09:05:06Z
title: Re-Hacked YouCompleteMe to Support Lamely Structured Projects
url: /blog/2015/02/01/hacked-youcompleteme-to-support-lamely-structured-pojects/
---

Nearly 1 Year ago, I attempted to hack `YouCompleteMe` for clang code completion for a lamely structured project, that hacking worked, but the hacking was dirty. For background information, [here](http://stackoverflow.com/questions/21746668/configure-vim-youcompleteme-for-special-project-structure) is my origin question on StackOverflow and [here](/blog/2014/02/20/hack-youcompleteme-and-libclang-for-lamely-structured-project/) is the detail about my first dirty hack. 

Recently when I looked at this hacking again, I thought there should be a better way, especially after I found [this code snippet](https://gist.github.com/Rip-Rip/758615) from GitHub Gist. At the moment I saw the code, I had a strong feeling that I have found the solution, I just need to verify it. To make code completion success for `clang -cc1 -fsyntack-only -code-complete-at a.c:4:11 main.c`, the source file to create `CXTranslationUnit` should be `main.c` and the source file to do `clang_codeCompleteAt` should be `a.c`. So I modified the code to pass one additional command line argument to be the file to create `TransicationUnit`, then compile and run command:

    $ ./complete a.c 11 4 main.c

When I saw the output, I confirmed this was a better way to hack `YouCompleteMe` for my need. 

Now I found the correct direction, the rest was just spend time to code, test and debug, here is a summary for what I have modified:

* File `cpp/ycm/ClangCompleter/TransicationUnit.cpp`, function `TranslationUnit::CandidatesForLocation`, take additional parameter `compfile` to pass to `clang_codeCompleteAt`
* File `cpp/ycm/ClangCompleter/CodeCompleter.cpp`, function `ClangCompleter::CandidatesForLocationInFile`, take additional parameter `compfile` and pass this newly added parameter to `unit->CandidatesForLocation`
* File `ycmd/completers/cpp/clang_completer.py`, function `ComputeCandidatesInner`, follow the way `_FlagsForRequest` is implemented, add new function to get the `parent` source file to create `TransicationUnit` and pass that result to `CandidatesForLocationInFile`
* File `ycmd/completers/cpp/flags.py`, add the new function to get `parent` source file
* File `.ycm_extra_conf.py` under project source directory, add function `ParentForFile`, this function read input `.clang_input_file`, which is the mapping for **code-complete-at-file** to **TransicationUnit-file**
* Created a python script to generate the mapping file for **code-complete-at-file** to **TransicationUnit-file**

Comparing to my first hacking, this round is very clean, only clang related part on `ycmd` is modified, the client side remain untouched. 

I have checked in the modifications to [my fork of `ycmd`](https://github.com/yongzhy/ycmd) at branch `lamely`. 
