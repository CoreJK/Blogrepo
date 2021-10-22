---
title: "Python类学习-静态方法与类方法"
date: 2021-10-22T14:35:46+08:00
description: "静态类 @staticmethod 和类方法 @classmethod 两个装饰器的基本使用场景。"
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

刚刚拜读了一篇在 python 类中：

静态类 `@staticmethod` 和类方法 `@classmethod` 两个装饰器的基本使用场景。   

收获良多，详细的分析可以阅读原文[^ 1]  

本文记录下使用静态类和类方法，在使用的细节，以及自己一些思考。

## 一、静态类的定义与使用

在学习 `@staticmethod` 之前，一直都是用『**私有方法**』来表示『**静态方法**』

### 定义静态类  

如代码所示的`_add_two_string_num` 方法，就是『私有方法』：

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    def _add_two_string_num(self, a, b):
        """在方法名前加一个下划线，用以表示私有方法"""
        a_int = int(a)
        b_int = int(b)
        return a_int + b_int
    
    def calc_age_after_n_year(self, n):
        age = self.add_two_string_num(self.age, n)
        print(f'{n}年以后，我{age}岁')
# 使用
>>> corejk = Person("core", "20")
>>> corejk.calc_age_after_n_year(10)
>>> 10年后，我30岁
    
```

用『静态类』改写一下，然后对比执行结果

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    @staticmethod
    def add_two_string_num(a, b):
①       """注意这里参数去掉了self"""
        a_int = int(a)
        b_int = int(b)
        return a_int + b_int
    
    def calc_age_after_n_year(self, n):
        # 注意这里把 self 替换成了 Person 类名
②       age = Person.add_two_string_num(self.age, n)
        print(f'{n}年以后，我{age}岁')
# 使用
>>> corejk = Person("core", "20")
>>> corejk.calc_age_after_n_year(10)
>>> 10年后，我30岁
```

可以看到代码执行结果还是一样  ，分析一些注释部分发生了什么：

​		① 我在 `add_two_string_num` 方法的下划线，然后再头上戴了一个静态方法的『`@staticmethod`』的帽子，并去掉了`self`形参    

​		② 戴上静态方法的帽子后，`add_two_strinig_num` 调用方法由 `self.xxx` 变成了`Person.xxx`， 使用类名调用该方法。

### 使用场景

仔细观察`add_two_string_num`这个静态类方法，可以发现它和 `Person` 这个类表示『人』这个抽象概念，没有什么关系（只是将两个字符串转成整型后相加）。  

如果代码的其他类也会用到它，可以把它单独放到 `Person` 类外面，作为一个『工具函数』;

如果这个『工具函数』只有某一个类会用到，那么最好给它带上`@staticmethod`的帽子(装饰器)。

> 一句话总结：静态方法就是某个类专用的工具函数。



## 二、类方法的定义与使用

> **此『类方法』非彼『类的方法』**，注意概念的区分

### 定义类方法

```python
import re

class People:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def introduce_myself(self):
        print(f'大家好，我叫: {self.name}')

    @staticmethod
    def add_two_string_num(a, b):
        ... skip ...

①   @classmethod
②   def from_chinese_string(cls, sentence):
        name = re.search('名字：(.*?)，', content).group(1)
        age = re.search('年龄：(\d+)', content).group(1)
③       return cls(name, age)

# 使用        
  >>> content = '我的名字：coreJK，我的年龄：20，把它提取出来'
④ >>> corejk = People.from_chinese_string(content)
  >>> corejk.introduce_myself()
  >>> 大家好, 我叫: corejk
```

​	①『类方法』要戴上`@classmethod`的帽子，而定义一个普通的『类的方法』不用；

​	②『类方法』除了要戴帽子，`self`也要换成`cls`，这个参数其实就是`People`这个类本身；

​	③ 这里相当于`People(name, age)`，但是还未实例化，并返回；

​	④ 在初次实例化`People`时，调用了类方法`from_chinese_string`，利用正则匹配`__init__`需要的`name、age`参数，完成；

> 这样做有什么好处呢？好处就在于我们完全不需要修改`__init__`  
>
> 那么，**也就不需要修改代码里面其它调用了`People`类的地方。**  
>
> 例如现在我又想增加从英文句子里面提取名字和年龄的功能，那么只需要再添加一个类方法就可以了：  

```python
import re

class People:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def introduce_myself(self):
        print(f'大家好，我叫: {self.name}')

    @staticmethod
    def add_two_string_num(a, b):
        ... skip ...

    @classmethod
    def from_chinese_string(cls, sentence):
        name = re.search('名字：(.*?)，', content).group(1)
        age = re.search('年龄：(\d+)', content).group(1)
        return cls(name, age)
	@classmethod
    def from_english_string(cls, sentence):
        name = re.search('name: (.*?),', content).group(1)
        age = re.search('age: (\d+)', content).group(1)
        return cls(name, age)
        

# 使用        
>>> content = 'My name：coreJK，My age：20，hello'
>>> corejk = People.from_chinese_string(content)
>>> corejk.introduce_myself()
>>> 大家好, 我叫: coreJK
```



### 使用场景

虽然可以额外写一些功能代码，去处理传入`people`类的初始化参数  

但是在实际项目中，会使代码的可读性、维护性降低（试想在一页几千行的代码中，找到处理的函数...）

也不符合 pythonic，不是么？

## 结语

工作中 pycharm 总是提示我

> Method 'xxx' may be 'static'

今天终于明白了 pycharm 的良苦用心... ...

[^ 1]: [未闻Code公众号，作者kingname](https://mp.weixin.qq.com/s/ssVqlcabRZU2V58WsL0FaA)
