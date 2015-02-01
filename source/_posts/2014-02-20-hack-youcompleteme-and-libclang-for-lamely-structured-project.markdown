---
layout: post
title: "Hack YouCompleteMe And libclang For Lamely Structured Project"
date: 2014-02-20 17:23:59 +0800
comments: true
categories: [vim, clang]
author: Zhu Yong
---

On 13 Feb 2014 I asked a question on [StackOverflow](http://stackoverflow.com/questions/21746668/configure-vim-youcompleteme-for-special-project-structure) on how to configure / modify vim plugin `YouCompleteMe` to fit special project structure. I guess my project structure is really lame, after two days waiting, some comments were given, but my problem not realy addressed. So I think I have to **DO IT MYSELF**. Then I read the source code for `YouComeplteMe` as well as `libclang`, because I only need the code completion function, lucky the related code are not much. Then I done  experiments with different approaches, and today, finally, I had my hacking works. Then I think I should record this nice experience, that's the purpose of this blog post. 

### My Question On StackOverflow

My C project has following structures. This is the structure for a large project, I can't change the structure. I want to use YouCompleteMe for semantic code completion for this project.

    main/                     // folder for C file to be compiled
      |- module1.c            // module main C file.
      |- module2.c
      |- .....
    module1/
      |- mod1_func1.c         // function file to be included in main module C file.
      |- mod1_func2.c
      |- mod2_func3.c
    module2/
      |- mod2_func1.c
      |- mod2_func2.c
      |- mod2_func3.c

Content for `moduleX.c`, this will include all related header files and module related C files.

    #include "header1.h"
    #include "header2.h"
    ...
    #include "modX_func1.c"
    #include "modX_func2.c"

Content for `modX_funcX.c` has one or few function definitions. Doesn't have header included

    // no header file included here
    int modX_funcX(void) {.....}

Because there is not related header included, `clang` must parse `moduleX.c` in order to do code completion on `modX_funcX.c`,  I have tried `clang` code completion from command line. Command below works

    clang -cc1 -x c -fsyntax-only -code-completion-at mod1_func1.c:4:11 module1.c 

So my question: how to configure YouCompleteMe to do code completion when I edit the `modX_funcX.c` file? 

I guess modification to YouCompleteMe source might required to do this job. My current idea is to add a file mapping database with format:

    path_of_file_to_complete:path_of_file_for_clang_to_parse

So before send the code completion request, get `path_of_file_for_clang_to_parse` from database based on current buffer name, pass this file name to `libclang`. 

Is my idea workable? If Yes, where is exactly place to add this file mapping function? 

<!-- more -->

### My Analysis

To make job easy, I make a simple project structure to simulate my rare project structure. 

    $ cd ~/dev/c/clang
    $ ls
    a.c  a.h  b.c  b.h  c.c  c.h  main.c

content for `a.h`

    typedef struct A {
        struct A * next;
        unsigned int data;
    }A;

content for `a.c`

    int test(void)
    {
        A *list;
        list->
    }

content for `main.c`

    #include "a.h"
    #include "b.h"
    #include "c.h"
    
    #include "a.c"
    #include "b.c"
    #include "c.c"

My ultimate goal is to let `YouCompleteMe` to do code completion when I type `->` at line 4 of file `a.c`. To test with `clang` from command line

    $ clang -cc1 -x c -fsyntax-only -code-completion-at a.c:4:11 main.c
    COMPLETION: data : [#unsigned int#]data
    COMPLETION: next : [#struct A *#]next

#### Analyze clang Command Line

So command line code completion works for this simulation structure. To understand why `clang` CLI works while `YouCompleteMe` doesn't, I need to find the difference at the point of code completetion for CLI and `libclang`. So I need a debug version of `clang`. Below are the steps to download and build debug version of `clang` version 3.3.

    $ mkdir -p /data/sourcecode
    $ svn co http://llvm.org/svn/llvm-project/llvm/tags/RELEASE_33/final llvm3.3
    $ cd llvm3.3/tools
    $ svn co http://llvm.org/svn/llvm-project/cfe/tags/RELEASE_33/final clang
    $ cd ../projects
    $ svn co http://llvm.org/svn/llvm-project/compiler-rt/tags/RELEASE_33/final compiler-rt
    $ cd /data/build
    $ mkdir llvm3.3-linux
    $ cd llvm3.3-linux
    $ /data/sourcecode/llvm3.3/configure --prefix=/data/software/C-C++/llvm3.3-hack/ CC=/usr/bin/gcc CXX=/usr/bin/g++
    $ make -j4
    $ make install

Next step is to load debug version of `clang` with the working arguments. I want to break at function `ASTFrontendAction::ExecuteAction()` to check some variables.

    $ gdb --args /data/software/C-C++/llvm3.3-hack/bin/clang -cc1 -x c -fsyntax-only -code-completion-at a.c:4:11 main.c
    
    (gdb) break z:\sourcecode\llvm3.3\tools\clang\lib\frontend\frontendaction.cpp:457
    
    Breakpoint 1 at 0xd775d3: file /data/sourcecode/llvm3.3/tools/clang/lib/Frontend/FrontendAction.cpp, line 457.
    
    (gdb) run
    
    Starting program: /data/software/C-C++/llvm3.3-hack/bin/clang -cc1 -x c -fsyntax-only -code-completion-at a.c:4:11 main.c
    [Thread debugging using libthread_db enabled]
    Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
    Breakpoint 1, clang::ASTFrontendAction::ExecuteAction (this=0x513a5d0) at /data/sourcecode/llvm3.3/tools/clang/lib/Frontend/FrontendAction.cpp:457
    457       if (hasCodeCompletionSupport() &&
    
    (gdb) print CI.getFrontendOpts().Inputs[0]
    
    $3 = (clang::FrontendInputFile &) @0x513b600: {File = {static npos = <optimized out>, _M_dataplus = {<std::allocator<char>> = {<__gnu_cxx::new_allocator<char>> = {<No data fields>}, <No data fields>}, _M_p = 0x513b5e8 "main.c"}},
    Buffer = 0x0, Kind = clang::IK_C, IsSystem = false}
    
    (gdb) print CI.getFrontendOpts().CodeCompletionAt
    
    $4 = {FileName = {static npos = <optimized out>, _M_dataplus = {<std::allocator<char>> = {<__gnu_cxx::new_allocator<char>> = {<No data fields>}, <No data fields>}, _M_p = 0x513b568 "a.c"}}, Line = 4, Column = 11}

From `gdb` output, I know `a.c` is stored in `CI.getFrontendOpts().CodeCompletionAt` while `main.c` is stored in `CI.getFrontendOpts().Inputs[0]`

#### Analyze YouCompleteMe with libclang

Entry point for code completion from `libclang` is `clang_codeCompleteAt` and eventually it call to `ASTUnit::CodeComplete()`. And at end of this function, it also call `ASTFrontendAction::ExecuteAction()` same as `clang` from command line. It also has input file defined by `Clang->getFrontendOpts().Inputs[0]`. Up to this point, my thought become clear, my target is how to make `Clang->getFrontendOpts().Inputs[0]` set to `main.c` with all the passed in parameters. 

### My Hacking

#### The First Fail Hack

My first attemp is rather simple, as I stated in my [StackOverflow question](http://stackoverflow.com/questions/21746668/configure-vim-youcompleteme-for-special-project-structure). When call `clang_codeCompleteAt()`, pass `complete_filename` in form of `path_of_file_to_complete:path_of_file_for_clang_to_parse`. This attempt failed because it won't even run to the point of `ASTUnit::CodeComplete()`. Along the calling path, there are other function check and parse the input file, my hacking filename format is an invalid filename for those functions.

#### The Second Fail Hack

I understand that `Clang->getFrontendOpts()` is parsed from the passed flags, so my second attempt of hacking is to pass `main.c` together with flags. So I hardcode `.ycm_extra_conf.py` to append absolute path of `main.c` in `flags`, but `Clang->getFrontendOpts().Inputs[0]` was not set to `main.c` when I check at `ASTUnit::CodeComplete()`. So this attempt failed.

#### The Third Successful But Dirty Hack

After failed previous two attempts, I only have the option to do dirty hacking by change `libclang` API and related functions, as well as related parts in `YouComplteMe`. The idea was to pass in additional `input_filename` together with `complete_filename`. And at `ASTUnit::CodeComplete()`, if this `input_filename` is valid, use it to replace `Clang->getFrontendOpts().Inputs[0]`. This attempt works!!!


### Conclusion 

To solve my problem, I was forced to read source code of `YouCompleteMe` and `clang`. The hacking process takes time because of code reading, compilation and debugging, and it's disappointed to find out my attempt didn't work as expected. But I really enjoy the moment when the code worked after some days' struggling. This whole hacking process is just another proof that open source is really good, you can do any hacking to fit your own need.

### Update 2015-02-01

I have tried another hacking, a better hacking, click [here](/blog/2015/02/01/hacked-youcompleteme-to-support-lamely-structured-pojects/) for more info.
