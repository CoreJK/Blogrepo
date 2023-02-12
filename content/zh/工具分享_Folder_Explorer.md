---
title: "如何用 Folder-Explorer 优雅地生成带注释的目录树?"
date: 2023-02-12T22:46:22+08:00
description: "有没有一个工具，可以满足以下几个需求：1.扫描目录，分析文件结构；2. 给任意文件添加备注； 3. 然后导出，带注释的，树形文本图形化工具呢？试试这个开源工具 Folder-Explorer 吧！"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "Folder-Explorer"
categories:
- "分享"
series:
- "工具"
image:
---

![Folder-Explorer](https://s2.loli.net/2023/02/12/m2EStDlQnsIzgo6.png)

## 前言📃

有没有一个工具，可以满足以下几个需求：

1. 扫描目录，分析文件结构
2. 给任意文件添加备注
3. 然后导出，带注释的树形文本的图形化工具呢？

在我脑海里最先想到的

是命令行工具 `tree`

但是它只能简单的生成目录树

仍然需要重新，继续编辑，不能记住我上一次的编辑内容

运气不错，在掘金的网站上，找到了技能满足上述需求，支持 Win/Mac/Linux 平台

使用简单，颜值还不错的开源软件 `Folder-Explorer`

## ✨Folder-Explorer✨

介绍一下 Folder-Explorer 基本的功能

> 1. 扫描目录;
> 2. 可视化编辑注释；
> 3. 导出美观的结构图（txt、json、xml、html）； 
> 4. 可以隐藏我希望忽略的文件

先展示一下导出效果吧：

```txt
├─LICENSE ----------------- // MIT License
├─pyproject.toml ---------- // 用于 pyinstaller 打包的配置文件
├─README.md --------------- // PyChatGPT 模块的介绍文档
└─src --------------------- // 源码目录
  ├─pychatgpt 
  │ ├─classes 
  │ │ ├─chat.py ----------- // 机器人互动接口
  │ │ ├─exceptions.py ----- // PyChatGPT 模块自定义的异常处理类
  │ │ ├─headers.py -------- // PyChatGPT 机器人User_Agent 请求头
  │ │ ├─openai.py --------- // OpenAPI 接口相关的代码
  │ │ └─spinner.py -------- // PyChatGPT 爬虫线程
  │ ├─main.py ------------- // 程序主入口
  │ ├─requirements.txt ---- // 项目依赖模块清单
  │ └─__init__.py 
  └─__init__.py 
```


下图是软件作者提供，软件的界面说明
![Folder-Explorer 界面图片来自掘金社区](https://s2.loli.net/2023/02/12/V39aOcMQ8Zle6Pt.png)

软件的使用十分直观，上手就能使用

我以最近在研究学习的 `PyChartGPT` 机器人模块项目为例子

用 `Folder-Exporer` 生成一份对 `PyChartGPT` 项目的目录树

## 一、偏好设置⚙️

在扫描之前，我们需要在软件的`偏好设置`里，屏蔽一些不需要扫描的目录

![屏蔽不需要扫描的目录](https://s2.loli.net/2023/02/12/7nNFuZfGPLzVcjM.gif)

如果有需要，可以在【通用】中，添加不需要扫描的文件、或者目录

![设置忽略的目录与文件](https://s2.loli.net/2023/02/12/vL4Ch5NmaxHJyer.png)

> 如果目录中，有这些无关目录，建议先配置上述内容
>
> 再开始进行下面的【扫描目录】操作，否则接下来【扫描目录】可能会崩溃或假死!


## 二、扫描目录🗂️

完成【偏好设置】以后，我们可以选择目录开始【扫描】了
![扫描目录](https://s2.loli.net/2023/02/12/Z2YWRNgpsFEuC3L.gif)

## 三、编辑注释🗒️
然后，就可以愉快的开始对目录、文件

进行注释的填写了
![Folder-Explorer 演示](https://s2.loli.net/2023/02/12/v5GhAeu2SFt3xZN.gif)

## 四、导出文本📦

在导出之前，复制下面的内容，设置导出的文件名称

```
项目结构注释-{YYYY}-{MM}-{DD}-{HH}-{mm}-{ss}
```

> 默认的导出模板，对于时间的分割，会导致文件名异常！

![设置导出的文件名称](https://files.mdnice.com/user/28994/b0b1e02b-f3c8-4354-981c-daed166dbd17.png)

`Folder-Explorer` 会按照导出时间，拼接出如下文件后缀

有了时间，就能清晰知道目录树的生产时间，便于进行版本控制

```
项目结构注释-2023-02-12-22-03-51.txt
```

导出操作演示：
![导出txt文本格式](https://s2.loli.net/2023/02/12/YidXWNEhCZwVtko.gif)

新鲜出炉的目录树！

```txt
├─LICENSE ----------------- // MIT License
├─pyproject.toml ---------- // 用于 pyinstaller 打包的配置文件
├─README.md --------------- // PyChatGPT 模块的介绍文档
└─src --------------------- // 源码目录
  ├─pychatgpt 
  │ ├─classes 
  │ │ ├─chat.py ----------- // 机器人互动接口
  │ │ ├─exceptions.py ----- // PyChatGPT 模块自定义的异常处理类
  │ │ ├─headers.py -------- // PyChatGPT 机器人User_Agent 请求头
  │ │ ├─openai.py --------- // OpenAPI 接口相关的代码
  │ │ └─spinner.py -------- // PyChatGPT 爬虫线程
  │ ├─main.py ------------- // 程序主入口
  │ ├─requirements.txt ---- // 项目依赖模块清单
  │ └─__init__.py 
  └─__init__.py 
```

如果不想导出后，立刻打开目录树文件

可以在设置里关闭

![关闭导出后立即打开](https://s2.loli.net/2023/02/12/BeZrN6Jk2CtwD3T.png)

## 写在后面

软件作者可能由于工作或者其他原因

没有继续维护这个好用的工具了

感谢提供如此易用，且友好的软件@d2-projets ！

开源万岁，希望自己未来也能写出这样的产品


## 项目与下载地址🖥️

1. [Folder-Explorer Github 下载地址](https://github.com/d2-projects/folder-explorer)
2. [Folder-Explorer Gitee 下载地址](https://gitee.com/d2-projects/folder-explorer)
3. [工具作者的介绍](https://juejin.cn/post/6844903954296340494)
