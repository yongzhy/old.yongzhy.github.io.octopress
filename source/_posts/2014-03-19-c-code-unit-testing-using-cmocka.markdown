---
layout: post
title: "C Code Unit Testing Using cmocka"
date: 2014-03-19 16:22:53 +0800
comments: true
categories: [c, testing]
authoer: Zhu Yong
---

[cmocka](http://cmocka.org/) is a unit testing framework for C, it is developed based on Google's project [cmockery](https://code.google.com/p/cmockery/). It was easy to use, and the author corrected some bugs from `cmockery`. 

### Install cmocka

First download source code for latest version of `cmocka` from [https://open.cryptomilk.org/projects/cmocka/files](https://open.cryptomilk.org/projects/cmocka/files). Current latest version is 0.3.2, and the source file is `cmocka-0.3.2.tar.xz`. `cmake` is required to build `cmocka`. 

    $ cd ~/Downloads
    $ xz -d cmocka-0.3.2.tar.xz
    $ tar vxf cmocka-0.3.2.tar
    $ cd cmocka-0.3.2
    $ mkdir build
    $ cd build
    $ cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release ..
    $ make
    $ sudo make install
    
### Use cmocka

Create source file with test cases. Simple example would be like:

    #include <stdarg.h>
    #include <stddef.h>
    #include <setjmp.h>
    #include <cmocka.h>
    static void test(void **state)
    {
        assert_int_equal(3, 3);
        assert_int_not_equal(3, 3);
    }
    int main(void) 
    {
        const UnitTest tests[] = 
        {
            unit_test(test),
        };
        return run_tests(tests);
    }

save file as `cmockatest.c

    $ gcc cmockatest.c -lcmocka -o cmockatest
    $ ./cmockatest

    [==========] Running 1 test(s).
    [ RUN      ] test
    3 == 3
    cmocka.c:8: error: Failure!
    [  FAILED  ] test
    [==========] 1 test(s) run.
    [  PASSED  ] 0 test(s).
    [  FAILED  ] 1 test(s), listed below:
    [  FAILED  ] test

### Reference

* [cmocka author's blog post](https://blog.cryptomilk.org/2013/01/14/cmocka-a-unit-testing-framework-for-c/)
* [Unit testing with mock objects in C](https://lwn.net/Articles/558106/)
* [cmocka API Documents](http://www.cmocka.org/api/)
