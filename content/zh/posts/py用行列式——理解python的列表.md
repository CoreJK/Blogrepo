---
title: "用行列式——理解python的列表"
date: 2019-07-22T16:03:09+08:00
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

## 前言
在学习python中的“列表”这个数据结构的时候，踩了许多坑。

总结一下如何处理**二维列表**
例如，我们要处理如下数据

``` python3
#! python3
Data = [['apples','oranges','cherries','banana'], ['Alice','Bob','Carol','David']， ['dogs','cats','moose','goose']]
```
表格里面包含了三个子列表，每一个子列表里，含有四个长度不一的字符串。
让我们换一种排版方式
### 这看上去不就是3行4列的矩阵么？
```python
#! python3
                0         1          2         3
     0      [['apples','oranges','cherries','banana'],
     1      ['Alice',   'Bob',    'Carol',  'David'],
     2      ['dogs',   'cats',   'moose',  'goose']]
```
- 行标： 0 ~ 2 是内层列表的在“母列表”索引下标
- 列标：0 ~ 3 是每个字符串，在内层列表中的索引下标


## 目标：
### 一、逐列输出（仍然是横向显示的），一列打印完后，换行继续打印

```python
  apples Alice dogs
 oranges Bob   cats
cherries Carol moose
  banana David goose
```
- **代码如下**
```python
#! python3
1 for col in range(len(data[0])): 
2	  for row in range(len(data)):
3		print(tableData[row][col], end = ' ')
4	  print(' ')
```
---
### 二、逐行输出，输出一行，换行继续打印

```python
apples oranges cherries banana
Alice Bob Carol David
dogs cats moose goose
```

- **代码如下**

```python
#! python3
1 for row in range(len(data)): 
2	  for col in range(len(data[0])):
3		print(tableData[row][col], end = ' ')
4	  print(' ')
```

## 总结
- 两段代码，其实只把第一行和第二行交换了。
- 一句话，逐行读取先for row，逐列读取就for col !
- 获取内层列表的下标，可以这样写
```python
for col in range(len(data[0])):
```

**当然，上面的例子比较特殊，内层列表的长度都是4**

