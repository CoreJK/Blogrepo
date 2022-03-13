---
title: "Joplin安装笔记"
date: 2022-03-13T11:19:29+08:00
description: "Joplin 保姆级安装&主题配置教程，搭建属于自己的信息管理大厦『地基』"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "Joplin"
categories:
- "信息管理"
series:
- "工具使用"
image:
---

# 前言

废话不多说，开始我们的 Joplin 安装之旅，也是我们迈开『信息&知识管理』的第一步
知识&信息的管理结构图，是我参考其他人的 Workflower 工作流，整理的V1.0版本
随着不断实践和完善，会不断更新。可以搜索我的公众号『QiuYuHouse 』获取。
Joplin 想要获取心仪的主题，需要考验你的动手能力，有手就行！

## 开箱安装

**下载 Joplin**

请点击☞[Joplin官网下载链接](https://joplinapp.org/download/)下载安装程序
不同平台得到的安装包不同，我以 win10 为栗~ 

![joplin安装](https://s2.loli.net/2022/03/12/jnmecNUtarDWbYK.gif)

## 语言设置为中文

完成后自动会自动打开，默认是英文的，通过`Tools-->General`
可以修改多国语言，中文也已经基本适配了。

![设置语言为中文](https://s2.loli.net/2022/03/12/J65FZA8YBVWXr2l.gif)

# 更换 Ohmine Dark Theme 主题

目前主题安装有两种方式
一种是通过 Joplin 内置的插件商店安装，目前只有三个
如果不想手动折腾主题，可以直接选择你喜欢的主题『安装』，然后重启即可
不过就失去了折腾的乐趣不是么~？

![主题插件](https://s2.loli.net/2022/03/12/Stl4WjJ7yMsdpUv.png)

另一种则是手动安装，也就是接下来的教程。
通过将别人分享的主题样式表文件，`userstyle.css` 和 `userchrome.css` 文件，复制粘贴进 Joplin 就好了

> 1. 如果你有 css 基础，完全可以自行设计主题，并分享给大家
> 2. 以后 Joplin 手动安装的主题，都可参考接下来的安装步骤

就两个文件而已\~不要有心理压力\~放开手脚去干吧！

## ① 下载主题文件

首先，去[Ohmine Dark Theme 的 Github 主页](https://github.com/Nacandev/Ohmine-Dark-Theme-For-Joplin)，下载最新的主题文件。
然后解压主题文件，到任意目录下『不用放到Joplin安装目录』，得到如下的文件内容：

![主题文件目录](https://s2.loli.net/2022/03/12/8M3mbzoRfwQVLJP.png)

不过我们只用关心『字体文件』、『主题 CSS 样式文件』即可

**三个字体压缩包**
- Font-CascadiaCode-2111.01.zip
- Font-ChironSansHKPro-ExtraLight.zip
- Font-Montserrat.zip

**重要！两个主题配置文件**
- userstyle.css
- userchrome.css

## ② 手动配置主题

`ctrl+a` 全选、`crtl+c` 复制 `userstyle.css` 中的代码，然后回到 Joplin 中，依次进入 `工具-->选项-->外观-->显示高级选项-->适用于已渲染 Markdown 的自定义样式表` 在弹出的编辑器内，粘贴我们刚从`userstyle.css`复制来的代码，并保存。

![复制 userstyle.css 中的代码](https://s2.loli.net/2022/03/12/xdbtfm4LireVvhj.gif)

`ctrl+a` 全选、`crtl+c` 复制 `userchrome.css` 中的代码，然后回到 Joplin 中，依次进入 `工具-->选项-->外观-->显示高级选项-->适用于 Joplin 全域应用样式的自定义样式表`  在弹出的编辑器内，粘贴我们刚从`userchrome.css`复制来的代码，并保存。

![复制 userchrome.css 中的代码](https://s2.loli.net/2022/03/12/hRH5yqlDNo74J8m.gif)

因为主题是暗色系列，所以还需要将 `选项-->外观-->主题` 改为『暗黑』，记得点击右下角的『应用』按钮哦！

![修改配色](https://s2.loli.net/2022/03/12/3bKgdoWfcIhTQ1s.png)

好了~
在替换完了两个 `css` 文件，并设置了配色后
**让我们重启 Joplin 看看效果**

> 以后修改了主题文件后，都需要重启，才能使修改生效

![成功](https://s2.loli.net/2022/03/12/HhLZqgwEQja1IdV.png)

到此，主题就替换成功了，你可以先欣赏一会儿，也可以继续来进行下一步。

## ③ 安装字体文件

既然是主题，那就少不了字体。
为了获得更好的码字体验
这个主题，需要系统安装三种字体包，都在我们刚才解压的文件夹内，将他们分别解压出来，然后逐个安装。

> 一个个点击安装字体确实是个痛苦的过程
> 有没有『一键安装』字体的办法呢？
> 请参考我的另一篇文章『在写了（抱头）』，借助`Fontreg`命令行工具，以后就能轻松地批量安装字体，而不用一个个点击字体安装了~

安装完成字体后，再重启 Joplin ，便可愉快的码字了，感受 `Ohmine-Dark` 主题的魅力吧！

# 尾声

你以为结束了？
不，对于 Joplin 的探索才刚刚开始。
后续我会继续补充 Joplin 的日常使用体验以及技巧。
另外，还有一些不错的[主题 Awesome-Joplin](https://github.com/kot-behemoth/awesome-joplin)，有兴趣的也可以按照我上文的教程，自行替换即可。

> 工具千万条，实用和安全第一条。
> Joplin 是我目前作为『信息管理』体系中的最后一环，用来积累、沉淀所接收知识的工具，起到类似数据库的作用；
> 信息过滤、处理需要借助 RSS 订阅，来对抗各种咨询APP的推送算法，通过 RSS 订阅来整合获取信息的渠道，然后『取精去糟』，在类似 Joplin 的笔记类应用中归类，做信息的主人；
> 工具有很多，最重要的是先打造合适自己的工作流，不断迭代，切勿沉溺工具，而忽略了最初的目的
> 最后这是话是我用来提醒自己的，也希望这篇教程能够帮助到有需要使用 Joplin 的朋友
