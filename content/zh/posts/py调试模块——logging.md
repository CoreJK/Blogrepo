---
title: "调试模块——logging"
date: 2019-07-22T15:53:16+08:00
description: "《python让繁琐的工作自动化》笔记"
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

# 前言

在没有学 logging 日志模块之前

为了发现脚本程序中的的bug，通常会用下面几种办法来调试，检查变量值，或者捕获到一些难以察觉的错误

- print()

- raise Exception()

- try:
    raise Exception()
  except Exception as err:

- assert 表达式, "message"

***

## 实例

- 实例代码功能，计算“数的阶乘”；

```
#!python3
def factorial(n):
	total = 1
	for i in range(n + 1):
		total *= i
	return total

print('7! = ' + str(factorial(7)))
```
**但是代码中存在的问题是 range(n+1)，由于 range 函数，迭代生成 0 ~ n 的一组数
一开始 i 被赋值为 0，total * i 就等于零，会导致最终结果等于零，这显然是不正确的**
**正常代码结果**

```
5! = 1 * 2 * 3 * 4 * 5 = 120
```
**异常代码结果**

```
5! = 0 * 2 * 3 * 4 * 5 = 0
```
**现在来调试代码可能存在的问题，就要看 i 每一次的变量值，和 total 相应的结果。**

***

## logging 日志记录模块


### 一、基本用法

```
#!python3
import logging
logging.basicConfig(
					level=logging.DEBUG, 
					format="%(asctime)s - [%(levelname)s] - %(message)s"
                    )
#logging.disable(logging.CRITICAL)
logging.debug('程序开始')

def factorial(n):
	logging.debug('factorial函数开始运行(%s)' % (n))
	total = 1
	for i in range(1, n + 1):
		total *= i
		logging.debug('i 为 ' + str(i) + ', total 为 ' + str(total))
	logging.debug('factorai函数运行结束(%s)' % (n))
	return total

print('7! = ' + str(factorial(7)))
logging.debug('程序结束.')

```

**运行效果**

```
2019-07-16 20:28:45,072 - [DEBUG] - 程序开始
2019-07-16 20:28:45,073 - [DEBUG] - factorial函数开始运行(5)
2019-07-16 20:28:45,073 - [DEBUG] - i 为 0, total 为 0
2019-07-16 20:28:45,073 - [DEBUG] - i 为 1, total 为 0
2019-07-16 20:28:45,073 - [DEBUG] - i 为 2, total 为 0
2019-07-16 20:28:45,073 - [DEBUG] - i 为 3, total 为 0
2019-07-16 20:28:45,073 - [DEBUG] - i 为 4, total 为 0
2019-07-16 20:28:45,073 - [DEBUG] - i 为 5, total 为 0
2019-07-16 20:28:45,073 - [DEBUG] - factorai函数运行结束(5)
5! = 0
2019-07-16 20:28:45,074 - [DEBUG] - 程序结束.
```

所以，我们可以看出，i 变量的数值从一开始就是 0 ,所以我们尽快定位问题所在。
并修改代码为

``` 
for i in rang(1, n+1):
```

### 二、日志级别(由低到高)
|级别|函数调用|描述|
|:-----:|:----:|:----:|
| DEBUG | logging.debug() |最低级别。用于小细节。通常只有在诊断问题时，你才会关心这些消息|
| INFO | logging.info() | 用于记录程序中一般事件的信息，或确认一切工作正常|
| WARNING  | logging.warning() | 用于表示可能的问题，它不会阻止程序的工作，但将来可能会|
| ERROR | logging.error() | 用于记录错误，它导致程序做某事失败 |
| CRITICAL | logging.critical() | 最高级别。用于表示致命的错误，它导致或将要导致程序完全停止工作|

### 三、将debug日志写到文件中

```
logging.basicConfig(filename='D:\\py\\Auto\\debug\\myProgramLog.txt',
					level=logging.DEBUG, 
					format="%(asctime)s - [%(levelname)s] - %(message)s")
```
将 filename 设置好后，所有 debug 消息就会写入外部的文本文件，命令行就不会显示了。

### 四、调试"开关" 
这个功能，才是以后不用 print 来调试的正真的关键

```
logging.disable(logging.CRITICAL)
```
通常将它，放在 loggin.basicConfig() 下一行，并且先注释掉。（可以看下基本用法中的例子）
调试完成后，取消注释，禁用所有调试信息输出。


## 总结
- logging.disable(logging.CRITICAL)，会禁止显示，当前级别，以及比当前级别低的调试信息。
- format 可以自定义消息输出的格式

```
logging.basicConfig(
                    level=logging.DEBUG, 
                    format="%(asctime)s - [%(levelname)s] - %(message)s"
                    )
```
**format 设置好后字符串格式后，如下输出**
```
2019-07-16 20:28:45,073 - [DEBUG] - i 为 0, total 为 0
```

```
%(asctime)s --> 2019-07-16 20:28:45,073
\-
[%(levelname)s] --> [DEBUG]
\-
%(message)s --> logging.debug('i 为 ' + str(i) + ', total 为 ' + str(total))
```