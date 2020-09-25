+++
date = "2019-07-22T16:07:25+08:00"
menu = ""
categories = ["编程学习笔记"]
tags = ["python"]

banner = "banners/code-python.jpg"
title = "os模块之——isfile、isdir、exists"
description = "《python让繁琐的工作自动化》笔记"
images = []
+++

昨天学到 os 模块中的两个函数<br>
可以用来检查，使用的文件是否存在于当前路径，以及是否有读取权限<br>
简要介绍用法，详细请参考python官方文档<br>
如有错误，欢迎指出。<br>
本人还是新手，望大佬包涵~

# 一、检查文件是否存在于当前路径

```
os.path.isfile(<filename>)
```
**如果存在，返回True，反之False**

# 二、检查当前文件是否具有读取权限

```
os.access(<filename>, os.R_OK)
```
**如果具有该权限，返回True，反之False**

# 三、例子

```
#! python2
import sys
import os

if len(sys.argv) == 2: 
	filename = sys.argv[1]
	if not os.path.isfile(filename): #如果，没有这个文件，则输出提示信息，并退出程序
		print "[-] " + filename + " does not exist."
		sys.exit()
	if not os.access(filename, os.R_OK): #如果文件不具有读取权限，则输出提示信息，退出程序
		print "[-] " + filename + " access denied."
		sys.exit()

	print "[+] Reading From: " + filename #以上两个条件都满足后，提示正在读取文件

else:
	print "[!] Useage " + str(sys.argv[0]) + " <filenam>" #如果没有输入脚本名称和文件，则提示使用方法

with open(filename, 'r') as f: #打开文件，并逐行读取，输出在屏幕上
	for line in f.readlines():
		print(line.strip('\n'))
```

# 四、总结
## 1. if not False :等价于 if True：

```
if not os.path.isfile(filename):
```
如果没有这个文件存在，则输出报错信息。
**正所谓双重否定，表示肯定True。**