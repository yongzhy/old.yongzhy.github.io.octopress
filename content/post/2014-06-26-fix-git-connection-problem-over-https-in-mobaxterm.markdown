---
authoer: Zhu Yong
categories:
- git
- MobaXterm
comments: true
date: 2014-06-26T11:12:10Z
title: Fix git connection problem over HTTPS in MobaXterm
url: /blog/2014/06/26/fix-git-connection-problem-over-https-in-mobaxterm/
---

[MobaXterm](http://mobaxterm.mobatek.net/) is a very nice portable ssh client as well as UNIX environment on Windows system. It has a lot of useful utilities including Xserver bundled into one single exe file. Recently I use it as my primary terminal on Windows system. But when I use the git plugin to clone / push my github repository, it first give me error of cert file not found, so I follow online instruction to set the root cert file in `~/.gitconfig` file to add following lines:

	[http]
		sslcainfo = c:/msysgit/mingw/bin/curl-ca-bundle.crt

This tells `git` to use the root cert file from `msysgit`, but next I saw new error messages:

    *   CAfile: c:/msysgit/mingw/bin/curl-ca-bundle.crt
    CApath: none

    ***
    
    error: The requested URL returned error: 403 Forbidden while accessing 

After tried with several solutions from Google search results, I found the solution provided by [GitHub](https://help.github.com/articles/generating-ssh-keys) solved my problem. The solution is simple, use SSH to replace the HTTPS connection. Below are the steps:

First, generate SSH key:

    $ ssh-keygen -t rsa -b 2048 -N ""

Next copy content from `~/.ssh/id_rsa.pub`, cdd public key to github account, 

    $ cat ~/.ssh/id_rsa.pub

Next, change the repository remote:

    $ git remote set-url origin git@github.com:yongzhy/vimconfig.git

Now it's DONE. 

