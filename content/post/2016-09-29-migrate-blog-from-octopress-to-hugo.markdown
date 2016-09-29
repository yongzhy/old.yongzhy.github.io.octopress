---
author: Zhu Yong
categories: 
- octopress
- hugo
comments: true
date: 2016-09-29T22:46:33+08:00
title: Migrate Blog From Octopress To Hugo
---

It's total 9 months since my last blog post, which was on 31 Dec 2015. It's really a very long time. :(

Today I am proud to announce I am back, and I just migrated the blog  from Octopress to Hugo. Reasons? 

First, Octopress no update for very long time, and I feel it's complicated to setup the environment when I switch to new system. Second, Hugo is written in Golang, single executable program, I love it, and I love Golang. Some more reasons??? Still need more? Those two reasons are enough for me to spend last 2 night to migrate from Octopress to Hugo. 

Migration is easy, but existing posts might requrie edit if it has image and blockquote. Because I want to keep the Octopress classic theme, I just follow the instructions from [here](https://parsiya.net/categories/migration-to-hugo/). After everything done, the blog pages looks almost same as Octopress version except there is no categories list in sidebar. 

I won't write down detail steps here, all steps are in the link above. But I need to mention that I use back my old Octopress repository, which is https://github.com/yongzhy/yongzhy.github.io, I backup the original Octopress `master` branch to `octopress_master`, original `source` branch to `octopress_source`, then replace all contents in `master` and `source` branch with content from Hugo. 

So to setup the blog environment on new system, I just need to 

1. Download hugo binary file from : https://github.com/spf13/hugo/releases
2. Install Pygments `sudo pip install Pygments`
3. Install https://github.com/john2x/solarized-pygment.git

    ```
    $ git clone https://github.com/john2x/solarized-pygment.git
    $ cd solarized-pygment
    $ sudo python setup.py install
    ```

4. Clone this repository

    ````
    $ git clone -b source https://github.com/yongzhy/yongzhy.github.io.git
    $ cd yongzhy.github.io.git
    $ git subtree pull --prefix=public https://github.com/yongzhy/yongzhy.github.io.git master
    ````

5. Add content

    ````
    $ hugo new post/newpost-title.markdown
    ````

6. Edit post
7. Generate And Publish

    ````
    $ hugo
    $ bash deploy.sh
    ````