---
title: "win10临时修改、永久cmd编码格式的方法"
date: 2020-03-17T19:14:17+08:00
description: "有时候cmd显示出现乱码，就需要修改相关编码、字体的设置了"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "CMD"
categories:
- "Windows"
series:
- "技术研究"
image: 
---

## 前言

有时候，运行一些命令行程序<br>
某些字符无法正常显示，常见的就是方块，或者是火星文字<br>
都是由于 cmd 程序的默认编码格式为 "GBK - 中文简体" 或其他编码格式，导致某些字体不能正常显示

## 一、临时修改
首先查看当前的活动代码页<br>
打开 cmd 输入chcp<br>
如果是以前从未修改过注册表，可能打开 cmd 后，输入 chcp 会提示如下

> 'chcp' 不是内部或外部命令，也不是可运行的程序
或批处理文件。

解决方法如下

### ① 打开注册表编辑器

**win + r，输入 regedit**

> 计算机\HKEY_CURRENT_USER\Console\%SystemRoot%_system32_cmd.exe

### ② 添加 CodePage (DWORD 32 位) 值
我添加的值 936 是可以显示中文的一种编码格式（GBK - 中文简体）<br>

这也是 windows10 中文系统默认的<br>

这里添加的目的，是为了以后可以临时手动修改

![添加CodePage值.gif](https://ae01.alicdn.com/kf/Ub67b0944acbd46099b2a6def235db89d3.gif)

通过修改注册表的这一项后，重新打开一个 cmd 窗口，输入 "chcp"<br>
就能显示当前的活动代码页面了（之前是无法使用 chcp 命令的）
![chcp执行结果.png](https://ae01.alicdn.com/kf/U15b6271b1d7248caa766ca5971a16265R.png)

现在，我们可以临时修改 chcp 为 65001 ，也就是 ‘UTf-8’ 的编码显示
![chcp临时改为65001.png](https://ae01.alicdn.com/kf/U9f08b3fafcd24df2ae1372cd2026ba57A.png)

当然，也可以改为你需要的活动代码页<br>
**重新打开一个 cmd 窗口，又会改为默认的 936 编码**<br>
但这只是权宜之计

# 二、永久修改

## ① 打开注册表编辑器

> 计算机\HKEY_CURRENT_USER\Software\Microsoft\Command Processor

## ② 添加 autorun 字符串值

![添加autoru字符串值](https://ae01.alicdn.com/kf/U1b112c8e04fd43cb91a7231dc8ba85d7P.gif)
现在无论你什么时候运行 cmd 命令行（哪怕是任意程序，调用 cmd 程序运行一些指令）<br>
都会默认使用 UTF-8 的编码显示了<br>

**注意！！！！**<br>
**如果需要显示特殊字体，修改编码后仍然无法正常显示，则需要额外安装命令行字体**

## 总结

1. 修改第一个注册表后，可以临时修改活动代码页，关闭窗口，或重启系统都会修改前的
2. 修改第二个注册表后，只要一运行 cmd 会自动修改活动代码页为 65001
3. 如果影响某些程序运行，可以删除这第二处注册表的值



