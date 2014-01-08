---
layout: post
title: "Blog Using Octopress On Multiple Computer"
date: 2014-01-08 14:29:20 +0800
comments: true
author: yongzhy
categories: Octopress
---

Last night when I tried to setup Octopress on my new bought Acer C720 chromebook, I found that Octopress website doesn't have the guide on how to setup Octopress on multiple computer. After search online, there are quite a lot people have seen the same problem. And I did found a very nice guide by Robert Anderson at [here](http://blog.zerosharp.com/clone-your-octopress-to-blog-from-two-places/). Here I will just list down the steps for my later on reference. 

clone `source` branch to local folder

    $ git clone -b source https://github.com/yongzhy/yongzhy.github.io octopress

clone `master` branch to *_deploy* subfolder

    $ cd octopress
    $ git clone https://github.com/yongzhy/yongzhy.github.io _deploy

configuration

    $ gem install bundler
    $ bundle install
    $ rake setup_github_pages

### To push changes from two different computers

Push everything on working computer before switch computer, using commands below:

    $ rake generate
    $ git add .
    $ git commit -am "commit message"
    $ git push origin source
    $ rake deploy

Then on the new computer, run commands below to sync.

    $ cd octopress
    $ git pull origin source
    $ cd ./_deploy
    $ git pull origin master
