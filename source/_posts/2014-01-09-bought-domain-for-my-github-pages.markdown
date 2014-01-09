---
layout: post
title: "Bought Domain For My Github Pages"
date: 2014-01-09 14:18:26 +0800
comments: true
categories: Octopress
author: yongzhy
---

Today I bought my first personal domain [zhuyong.me](http://zhuyong.me) and pointed to [my github pages](http://yongzhy.github.io).

The reason why I bought domain is to push myself, to encourage myself to continously update this blog. Plus, the domain + 1 year pravacy protection only cost US$8.99 under current promotion. 

The setup is pretty easy:

1. create `CNAME` file under `source/` folder. In this file, just input my newly bought domain: **zhuyong.me**
2. run `rake generate` to put `CNAME` in `master`
3. setup DNS, I bought my domain from [namecheap](http://www.namecheap.com). So just point `zhuyong.me` to `192.30.252.153`, and `192.30.252.154`, then point `www.zhuyong.me` to `yongzhy.github.io.`
4. that's all, just wait for less than 30mins, the domain is updated and I have `zhuyong.me` now pointed to my github pages.
