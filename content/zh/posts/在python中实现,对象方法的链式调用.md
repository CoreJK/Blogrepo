---
title: "在python中实现,对象方法的链式调用"
date: 2022-04-08T13:01:33+08:00
description: "在python中的类中，方法链式调用的写法"
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
- “技术研究”
image:
---

这个标题怎么都不太符合我想表的意思，等我想好下次再改吧。

## 前言

阅读项目代码的时候，发现了类`CreateHuman`在实例化为 Gen 对象后  
对于方法的调用是这样子的  

> Gen = CreateHuman()

> result = Gen().fun_A(arg).fun_B(arg2).create()

很好理解（其实Debug了好久，项目代码就像洋葱一样，边看边流泪...），翻译过来就是：  

> 先调用类Gen的 fun_A 方法做点什么，然后继续调用 fun_B 方法做点什么，最后调用 Create ，完成了Gen类的任务，得到了一个结果

我们看看源代码怎么实现的：

```python
class Gen(object):
	"""一个生成名字的类"""
	
	def __init__(self):
		"""实列化时暂时赋值为 None"""
		self.first_name = None
		self.last_name = None
		self.just_a_while = 0
		
	def get_first_name(self, first_name):
		"""获取 first_name 字符串，返回对象本生"""
		self.first_name = first_name
		return self
		
	def get_last_name(self, last_name):
		"""获取 last_name 字符串，返回对象本生"""
		self.last_name = last_name
		return self
	
	def create_name(self):
		full_name = self.first_name + ' ' + self.last_name
		return full_name
		
	def sleep_while(self):
		"""起名字很累的！"""
		self.just_a_while += 1
		print("让我眯一会儿\n" * self.just_a_while)
		# sleep(self.just_a_while)
		return self
		
		
if __name__ == '__main__':
	first = 'your'
	last = 'name'

	Generator = Gen()
	human_name = Generator.get_first_name(first).get_last_name(last).create_name()
	print(human_name)

>>> 'your name'
```

## 分析代码实现

注意 `get_first_name` 和 `get_last_name` 方法返回的内容，不是`变量`或者`表达式`，而是 `self` 

这意味着，当对象 `Generator` 调用了 `get_first_name` 后  
又能够继续『调用』自己的其他方法『或者再次调用 `get_first_name`』，就像九所连环那样  

再调用 get_first_name 意义不大  
我们让这个对象休息一会儿  
毕竟给人起名字是件很费脑子的事情，给变量起名也是！  

来看看 `sleep_while` 方法被调用多次会怎么样~  

```python

Gen.sleep_while().sleep_while().sleep_while()

>>> 让我眯一会儿

>>> 让我眯一会儿
>>> 让我眯一会儿

>>> 让我眯一会儿
>>> 让我眯一会儿
>>> 让我眯一会儿

```

这里『链式』调用了 sleep_while() 方法 3 次，每一次 睡眠的次数 都会 `+1`

**像不像早上摁掉闹钟的你，一遍又一遍的按下再睡一会儿『just_a_while』... ...**

## 思考

其实在平常用一些 python 的内置方法的时候，经常会对内置的数据对象  
例如：字符串的各种方法 `string.low().title()`，都可以这样链式调用  

> 万物皆对象啊！

以前只是默认接受了python 中，对象的方法可以链式调用的『设定』  
而这些内置对象的类，他们的方法，为什么可以这样调用，没有过多思考  

> 这样写有什么好处呢？

目前我只能想到，这样可以帮助简化代码，简化方法的调用逻辑（看上去  
不用一行行去调用类的方法，视觉上更加易读（雾  
以后熟练，再来补充实际应用场景吧  
