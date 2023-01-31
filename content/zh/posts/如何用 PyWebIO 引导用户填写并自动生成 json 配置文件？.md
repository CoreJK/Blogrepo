---
title: "如何用 PyWebIO 引导用户填写并自动生成 Json 配置文件？"
date: 2023-01-31T10:23:14+08:00
description: "你开发的了一个工具，工具在使用前需要配置一些参数 为此你详细的写了一份，详细到自己都不一定看的文档... ... 也许你可以试试 PyWebIO ！快速构建友好的配置填写前端~"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "python"
categories:
- "编程语言"
series:
- "技术研究"
image:
---


## 前言

当你写好了一个自动化工具，准备分享给你的同事或者朋友使用

你详细的写了一份文档，并进行了一次分享，告诉大家要如何配置这个工具

但是，只要是输入，就有出错的可能

能不能在提供一个简单的引导，来一步步的辅助工具的使用者，正确的配置工具的使用参数呢？

这样既能减少使用者的学习成本，也能减轻工具开发者的维护成本

也许你可以试试它

## pywebio 

简单介绍一下

> PyWebIO 提供了一系列命令式的交互函数来在浏览器上获取用户输入和进行输出，将浏览器变成了一个“富文本终端”，可以用于构建简单的 Web 应用或基于浏览器的GUI应用。 
>
> 使用 PyWebIO，开发者能像编写终端脚本一样(基于input和print进行交互)来编写应用，无需具备 HTML 和 JS 的相关知识；
>
> PyWebIO 还可以方便地整合进现有的 Web 服务。非常适合快速构建对 UI 要求不高的应用。

官方提供了一个用 pywebio 构建的 【BMI 体脂计算器】 [演示站点](https://pywebio-demos.pywebio.online/bmi)

![pywebio Demon](https://pywebio.readthedocs.io/zh_CN/latest/_images/demo.gif)

## 特性

> 1. 使用同步而不是基于回调的方式获取输入，代码编写逻辑更自然
> 2. 非声明式布局，布局方式简单高效
> 3. 代码侵入性小，旧脚本代码仅需修改输入输出逻辑便可改造为 Web 服务
> 4. 支持整合到现有的Web服务，目前支持与Flask、Django、Tornado、aiohttp、 FastAPI(Starlette)框架集成
> 5. 同时支持基于线程的执行模型和基于协程的执行模型
> 6. 支持结合第三方库实现数据可视化

我们尝试实现一个简单的应用场景吧


## 借助 pywebio 生成一份 json 配置信息

```python
#!python3
# gen_json.py - 借助 pywebio 正确快速的配置 json 等格式的配置文件

from pywebio.input import *
import json

# 假设需要生成一份服务器 SSH 的连接配置信息
host_info = input_group("服务器信息", [
    input('服务器IP', name='ip'), 
    input('SSH 用户名', name='user_name'), 
    input('SSH 密码', name='passwd'), 
    input('端口号', name='port')
    ])

# 调试信息，输出在终端
print(host_info['ip'], host_info['user_name'], host_info['passwd'], host_info['port'])

# 等待用户输入，输入完成以后，写入 seeting.json 配置文件
with open("settings.json", 'w') as json_file:
    json_file.write(json.dumps(host_info, indent=2))
    
```

## 生成效果

当我们运行 gen_json.py 文件后，浏览器弹出了一个窗口

![pywebio 生成 json 配置](https://s2.loli.net/2023/01/31/6D8Y7i4vAJZ1H9X.png)

工具的使用者，可以根据代码的指引，填入相应的信息

如果配置文件发生了变动，可以只用修改 input_group 输入组的内容



## 后记

当然 pywebio 不只有 input 输入框功能，这是官方手册提供的，基本涵盖了常见的输入场景：

| 函数                                                         | 简介         |
| ------------------------------------------------------------ | ------------ |
| [input](https://pywebio.readthedocs.io/zh_CN/latest/input.html#pywebio.input.input) | 文本输入     |
| [textarea](https://pywebio.readthedocs.io/zh_CN/latest/input.html#pywebio.input.textarea) | 多行文本输入 |
| [select](https://pywebio.readthedocs.io/zh_CN/latest/input.html#pywebio.input.select) | 下拉选择框   |
| [checkbox](https://pywebio.readthedocs.io/zh_CN/latest/input.html#pywebio.input.checkbox) | 勾选选项     |
| [radio](https://pywebio.readthedocs.io/zh_CN/latest/input.html#pywebio.input.radio) | 单选选项     |
| [slider](https://pywebio.readthedocs.io/zh_CN/latest/input.html#pywebio.input.slider) | 滑块输入     |
| [actions](https://pywebio.readthedocs.io/zh_CN/latest/input.html#pywebio.input.actions) | 按钮选项     |
| [file_upload](https://pywebio.readthedocs.io/zh_CN/latest/input.html#pywebio.input.file_upload) | 文件上传     |
| [input_group](https://pywebio.readthedocs.io/zh_CN/latest/input.html#pywebio.input.input_group) | 输入组       |
| [input_update](https://pywebio.readthedocs.io/zh_CN/latest/input.html#pywebio.input.input_update) | 更新输入项   |

pywebio 的参考文档十分详细，并且有中文翻译

当然，不止可以用于生成 json 配置文件 配合相关的模块 yaml、toml、ini 等配置文件，也能简单的引导用户配置


## 参考

1. [pywebio 官方文档](https://pywebio.readthedocs.io/zh_CN/latest/guide.html)
