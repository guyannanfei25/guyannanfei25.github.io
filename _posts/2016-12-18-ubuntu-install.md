---
layout: post
title: wubi 安装 Ubuntu 一些经验
categories: ubuntu
description: wubi 安装 ubuntu 经验
keywords: wubi, ubuntu, 经验
---

做服务端开发，在windows下编程总有点不习惯，自己笔记本上面的windows系统上面有好多历史资料，重新格式化完全抛弃不现实；重新分一个区来又有点捉襟见肘，因此使用wubi安装的方式，将ubuntu作为win下面的一个软件来最适合，安装起来最方便。下面简单说下安装方式和一些经验。

# wubi安装

自己的笔记本已经用好久了，虽然里面资料不多，但是不能一下全部格式化，笔记本买的比较早，很难单独分出一个区来，因此最简单的方式就是使用wubi安装，将ubuntu作为windows下的一个软件来管理。既有双系统的便利，又不用安装设置新的引导。以下就是安装步骤：

1， 在ubuntu官网上下载自己需要版本的iso，与wubi放在同一个文件夹下面。wubi可以解压对应的iso在里面找到，据说高版本的不提供wubi安装了，我在之前旧版本下面解压的。双击wubi，设置对应的参数，win下面安装软件的过程就不详细说了。

2， 然后就是等待重启自己安装完成，抽根烟去冷静下吧。

# 问题
* 安装完之后进不去图形化界面

> 之前在实验室的时候遇到过几次，以为是图形化软件崩溃了，网上搜了下对应问题都是让重装x window，但是安装软件时又提示只读权限，又是一大堆解决办法，很是麻烦；尝试了一会，发现各种不行，都没说到点子上，其实就是wubi安装的ubuntu，启动的时候将自己设置为只读启动，在进入启动选项的时候，按e进入，修改下启动选项为rw权限，进去之后将配置文件永久保存下即可。

* 进入之后ibus输入中文有问题


* 将终端显示改为相对路径

> refer:http://www.crifan.com/ubuntu_terminal_only_show_current_folder_path/
> .bashrc中PS1变量中\w改为\W

* 安装chrome

* 将chrome更改为bing搜索

* 启动工作区

* 禁用独显

* ThunderBird Block Remote Content in Messages

> 选择 Edit -> Perference -> Privacy -> Allow remote content in messages
