---
title: "开源免费！跨平台修改系统 hosts 文件好帮手SwitchHosts"
date: 2023-02-02T22:12:50+08:00
description: "虽然修改 hosts 并不难，有没有更加快捷（懒）的方法呢？可以快速的切换不同的 hosts 配置信息呢?最好是还能 windowns 、 linux 、 mac 多平台使用~毕竟服务器一多，总有忘记和填写错误的时候XD 今天的主角就是 SwitchHosts ，满足了所有需求!"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "SwitchHosts"
categories:
- "分享"
series:
- "工具"
image:
---

![](https://s2.loli.net/2023/02/02/pmJl3MseGYvFV68.png)
## 前言
作为软件开发人员，或者喜欢折腾本地电脑网络的人

一定对系统的 **hosts** 文件不会陌生，当我们需要对访问的网站链接，进行重定向
就会去修改它

在我的个人工作场景中
需要配置不同的 **TDengine** 数据库所在服务器的 **FQDN**，因此需要频繁的修改操作系统的 **hosts** 文件

虽然修改 hosts 并不难，有没有更加快捷（懒）的方法呢？可以快速的切换不同的 **hosts** 配置信息呢

最好是还能 **windowns**、**linux**、**mac** 多平台使用

毕竟服务器一多，总有忘记和填写错误的时候XD

今天的主角就是 **SwitchHosts**，满足了所有需求

## SwitchHosts

作为一个图形化的工具，少不了友好的交互界面
启动软件后，功能一目了然
- 左侧，是给我们自定义的不同 **hosts** 配置的地方，每一份配置，都有独立开关配置；
- 右侧，则是系统的 hosts  信息的编辑、展示区域；

![](https://files.mdnice.com/user/28994/6d74c4e4-cd7e-40c3-b461-bbbfdc9c9c9c.png)

### 功能特性

> - 友好的 **UI** 界面
> - 快速切换 **hosts** 方案
> - hosts 语法高亮
> - 支持从网络加载远程 **hosts** 配置
> - 可从系统菜单栏图标快速切换 **hosts**

## 添加自己的 **hosts** 配置

现在，我们可以配置不同的 **hosts** 信息了


![添加自己的 **hosts** 配置](https://s2.loli.net/2023/02/02/c71AzIVNHvGRgMY.gif)


## 快速切换 hosts

**SwitchHosts** 在界面关闭以后，会在后台运行

当我们需要切换不同的 **hosts** 配置时，只需要单机 **switcHosts** 的图标

选择不同的 **hosts** 配置就好了~

![任务栏快速切换](https://s2.loli.net/2023/02/02/xg4vODujawKRXhP.gif)

## 注意！**hosts** 文件的权限！

如果遇到下面的情况，不要慌

![文件没有写入权限](https://s2.loli.net/2023/02/02/C8EjA1iPTtLncIp.png)

我们只需要修改一下  **hosts** 文件的读取权限即可

**switchHosts** 贴心的为我们，提供了 **hosts** 文件的入口

![系统 hosts 文件入口](https://s2.loli.net/2023/02/02/boRaxpMtJe5K4d7.gif)

打开资源管理器，将 hosts 文件的权限的`只读`，取消勾选，不要打勾

记得点击`应用` 使修改生效

![取消勾选文件的权限的`只读`](https://s2.loli.net/2023/02/02/3UP7w6fNncvCojm.png)

## 覆盖还是追加，根据需要选择

写入 **hosts** 的逻辑有两种，一种是**完全覆盖**，另外是在原有的配置下方，**继续追加**

在我们第一次使用的时候，软件会提示我们选择**“追加”**还是**“覆盖”**，根据自己的需要选择即可

如果，想要改变写入模式我们可以【**设置**- -> **选项**】里进行修改，来满足不同的场景

![hosts修改模式](https://s2.loli.net/2023/02/02/Isbk9Cn2A4pe1HE.png)

**PS**：**如果遇到一些软件运行异常，请将下面的地址恢复到 hosts 文件中**

> 127.0.0.1 localhost
>
> 255.255.255.255 broadcasthost
>
> ::1 localhost


## 工具下载链接

截至这篇文章发布，SwitchHosts 发布到了 `4.1.1.6077` 

![发布版本](https://s2.loli.net/2023/02/02/FnTZWUep9AfMy4P.png)

参考资料链接请在浏览器复制打开~

1. [SwitchHosts 项目主页](https://github.com/oldj/SwitchHosts "SwitchHosts 项目主页")