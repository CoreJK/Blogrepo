+++
date = "2020-02-27T19:57:54+08:00"
menu = ""
categories = ["各种折腾"]
tags = ["linux"]

banner = "banners/screen.jpg"
title = "Screen命令的基础用法"
description = "Linux多命令行管理工具"
images = []

+++

# 前言

有时我们会使用 ssh 终端连接 linux 的云服务器，运行一些程序

但如果断开当前的命令行窗口，某些程序、任务就会终止运行

为了让程序、任务在我们断开 ssh 连接时，继续运行，这时我们就要用到 Screen 命令了

# 安装

没有默认安装的系统要先安装，我的系统是 **Ubuntu 16.04**

> apt-get install screen

# 基本用法

## Ⅰ、打开一个新的命令行窗口

给你想要后台运行任务、程序的命令行窗口，取一个好记的名字

然后就和往常一样，运行你的程序、任务就行了

> screen -dmS < task1 >


## Ⅱ、安全退出当前命令行窗口

退出后，会回到最初的命令行窗口，而你创建的新命令行窗口，会在后台继续运行

> Ctrl + a + d


## Ⅲ、查看后台，运行中的命令行窗口

故名思意，列出后台运行中的，命令行窗口的 ID、名字
> screen -list


## Ⅳ、切换指定命令行窗口
通过上一个命令，就可选择你要查看的运行中的窗口的ID、名字

> screen -r < task_name, ID>

## 五、彻底关闭命令行窗口
通过上面的指令，能够结束你不像运行的任务、程序的ID、名字

> screen -S < task_name, IDs> -X quit