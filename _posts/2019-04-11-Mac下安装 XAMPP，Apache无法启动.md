---
layout:     post
title:      "Mac下安装 XAMPP，Apache无法启动"
subtitle:   ""
date:       2019-04-10 16:00:00
author:     "xxspring"
header-img: "img/post-bg-2015.jpg"
tags:
    - Mac
---


## 解决Mac下安装 XAMPP，Apache无法启动

[原文链接](https://www.jianshu.com/p/988703317a30)

XAMPP版本是 5.6.15-2

apache无法启动的解决办法是

1.查看端口是否被占用

```
sudo lsof -i -n
```

2.用终端运行xampp，查看具体的错误信息

```
sudo su

/Applications/XAMPP/xamppfiles/xampp start

大部分是这个问题：

XAMPP: Starting Apache…fail.

XAMPP: Another web server is already running.

解决办法：

sudo apachectl stop

再重新启动apache看，是不是已经启动成功啦～
```
