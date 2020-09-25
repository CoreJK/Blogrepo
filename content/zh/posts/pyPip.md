+++
date = "2019-02-25T11:35:10+08:00"
menu = ""
categories = ["编程学习笔记"]
tags = ["python"]

banner = "banners/jeep.jpg"
title = "如何升级pip3"
description = "如何正确升级pip2和pip3"
images = []
+++

# 前景提要
电脑里安装了python2和python3两个版本。
为了方便在CMD里来回切换使用
<br/>我把位于

> C:/python3/python.exe

改成了

> python3.exe

<br/>这样就能在2和3之间来回切换了。
<br/>

## 升级pip3正确姿势如下

安装python模块，每次使用pip3 install 【模块】
会提示

1.升级提示
```cmd
"You are using pip version x.x.x, however version x.x.x is available.
You should consider upgrading via the 'pip install --upgrade pip' command"
```
2.正确姿势
```cmd
python3 -m pip install --upgrade pip
```
这样你就能正常的升级pip3了

# 写在后面
在windows下安装了python2,和python3后
<br/>pip版本对应关系如下

>pip=pip2

<br/>
pip3就是独一无二的存在