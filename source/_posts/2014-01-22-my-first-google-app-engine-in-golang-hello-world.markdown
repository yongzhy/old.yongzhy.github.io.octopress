---
layout: post
title: "My First Google App Engine In Golang : Hello World!"
date: 2014-01-22 17:34:05 +0800
comments: true
author: Zhu Yong
categories: [GAE, golang]
---

Long time ago, I was thinking to build something using Google App Engine. But I didn't start coding after created the app in GAE. So the apps remain there with a `None Deployed` status. 

Yesterday I downloaded the GAE Golang SDK, the reason for choosing experimental golang SDK is that, I want to be more familar with Golang though the process of GAE development.

So the process started with the the [`Hello World`](https://developers.google.com/appengine/docs/go/gettingstarted/helloworld) example. I created the folder and files, then when I issue `goapp serve` command, I saw error about the `app.yaml` file. It's because the indent of line 

    - url: /.*
    
After corrected the indent problem, the server started successfully. I need to mention that I ssh remote login my Ubuntu server to do the development. So after the `HelloWorld` app server started, I tested on my laptop with `http://1.2.3.4:8080` (assume my Ubuntu server IP is `1.2.3.4`). Chrome reported page can't found. That's strange, there is no error message from GAE, but there must be something wrong. 

So I use `goapp help serve`, then I found there is a `--host=` option, then I tried to set the host to ip `1.2.3.4`, bingo, I saw `hello world!`. 

After that I still wondering why I can't access the server using IP if I didn't append the `--host=` options. Then I found someone asked the same question on [StackOverflow](http://stackoverflow.com/questions/7534967/is-there-any-way-to-access-gae-dev-app-server-in-the-local-network) in Year 2011. I confirmed the answer of `--host=0.0.0.0` works same as `--host=1.2.3.4`. 

Next step I will finish the app I should finish long time ago. 