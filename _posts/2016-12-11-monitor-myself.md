---
layout: post
title: Monitor——一个轻量级监测软件的编写
categories: 开源
description: 我写的一个Linux下开源的流量监控程序
keywords: Linux, 开源, Monitor
---

&nbsp;&nbsp;&nbsp;&nbsp;为什么要写这个软件呢？很简单，之前的[Readme](https://github.com/guyannanfei25/BLOG/blob/master/Readme.md)已经说过了，因为实验室项目需要个直观的效果展示，虽然我们生活在一个伟大的时代：`互联网方便了我们的生活，降低了我们的学习成本和门槛，各种开源软件层出不穷`，但是何时我的竟然没有。最好用的就是iptraf。在Ubuntu任何源下面都可以下载的到。它有以下优点：

* 比较小巧
* 不使用图形化界面
* 能够比较实时的显示大部分网口的流量

&nbsp;&nbsp;&nbsp;&nbsp;但是*有多方便就有多麻烦*`我一直这么认为,比如为了保证程序始终存在给程序加上守护进程，对于程序员调试的时候简直是个噩梦`，iptraf的一下缺点使我下决心自己写一个：

* 不能够监测虚拟网卡的流量
* 对网卡动态的增加和删除支持性不好*好像根本不支持网卡动态增删*
* 未完。。。