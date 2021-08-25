---
title: "python合并列表为字典，并排序"
date: 2020-10-05T10:04:21+08:00
description: "lambda匿名函数帮了大忙"
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

昨天为了实现哈夫曼编码

遇到了如何合并两个列表、以及字典排序问题

## 整理数据

```python
chart = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k']
probability = [0.1, 0.08, 0.14, 0.05, 0.18, 0.03, 0.2, 0.008, 0.135, 0.017, 0.06]
```

为了后面数据的处理，需要上边的两个列表（字符和它出现的概率），合并为一个字典

```python
node_dict = dict(zip(chart, probability))
```

- 通过 zip 函数，两个列表内的元素，一一映射组成元组 （‘a’, 0.1)，('b', 0.08)... ...，
- 通过 dict 函数，转换为字典

```python
{'a': 0.1, 'b': 0.08, 'c': 0.14, 'd': 0.05, 'e': 0.18, 'f': 0.03, 'g': 0.2, 'h': 0.008, 'i': 0.135, 'j': 0.017, 'k': 0.06}
```

zip 函数得到数据并不能直接使用，可以用 list 函数转为一个元组列表，或者 dict 函数转为字典

> <zip object at 0x000001A37F34EE88>

## 字典排序

排序用到的函数就是 sorted 了（sort是列表的方法，字典对象是没有这个方法的，牢记！）

> sorted(iterable, /, *, key=None, reverse=False)
>
> *sort 是应用在 list 上的方法，sorted 可以对所有可迭代的对象进行排序操作。*

1. 如果直接给它一个字典，会默认按照 "键" 排序；
2. sort原地排序，sorted 会先复制原来的数据，重新生成一个排序的结果；
3. 默认 reverse = False 按照由小到大排序，需要从大到小，设置 reverse=True即可；

### 通过键排序

```python
node_dict = sorted(node_dict.items(), key=lambda k: k[0])
node_dict = dict(node_dict)
>>> {'a': 0.1, 'b': 0.08, 'c': 0.14, 'd': 0.05, 'e': 0.18, 'f': 0.03, 'g': 0.2, 'h': 0.008, 'i': 0.135, 'j': 0.017, 'k': 0.06}
```

### 通过值排序

```python
node_dict = sorted(node_dict.items(), key=lambda v: v[1])
node_dict = dict(node_dict)
>>> {'h': 0.008, 'j': 0.017, 'f': 0.03, 'd': 0.05, 'k': 0.06, 'b': 0.08, 'a': 0.1, 'i': 0.135, 'c': 0.14, 'e': 0.18, 'g': 0.2}
```

## 总结

具体发生了什么呢？

- node_dict.items() 获得了一个可以迭代的对象

```python
dict_items([('a', 0.1), ('b', 0.08), ('c', 0.14)... ])
```

- lambda 函数通过表达式 k: k[0] ，先索引 ('a', 0.1) 中的 'a'，于是 sorted 开始按照字符的 ASCII 码值开始排序
- lambda 函数通过表达式 v: v[1] ，索引了 ('a', 0.1) 中的 0.1，然后 sorted 开始按照数值大小开始排序；

zip，dict, sorted，lambda；

分别实现了列表的合并，字典转化，字典的排序，选取排序的依据