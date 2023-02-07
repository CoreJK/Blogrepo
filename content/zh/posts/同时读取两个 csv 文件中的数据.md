---
title: "python 利用生成器处理大于内存的 csv 文件数据"
date: 2023-02-06T22:25:43+08:00
description: "当两个 csv 文件总大小，大于计算机的运行内存，用 python 如何同时，循环读取两个 csv 文件中的一行数据？"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "编程语言"
categories:
- "python"
series:
- "技术研究"
image:
---

![](https://s2.loli.net/2023/02/06/gxQw1s8jSCEK9iP.png)

## 0x00 问题场景📃

现在你请想象，有这样一个运行环境，以及需要处理的文件:
1. 一台运行内存只有 8 GB 的电脑；
2. 两份 csv 文件大小分别为 16 GB；

python 代码要求实现：
1. 同时读取两份 CSV 文件中所有的 data 行的数据；
2. 将 data 数据，进行差值计算；
3. 最终将得到的所有行的，进行两两求和

```bash
# A.csv - 16GB
index, data
1, 100
2, 100
3, 100
...

# B.CSV - 16GB
index, data
1, 90
2, 90
3, 90
...
```

## 0x01 构建 csv 文件的生产器🏭

如果不用生成器，同时将两个 16 GB 的文件加载到只有 8GB 的内存中，势必会造成内存的溢出，所以我们需要先解决这个问题

```python
import csv

def get_csv_data(csv_file_name, jgnore_header=False):
  try:
    with open(csv_file_name, encoding='utf-8') as csv_file:
      data_reader = csv.reader(csv_file)
      # 有些 csv 文件需要跳过表头
      if ignore_header == True:
        next(dataReader)
      for one_line in data_reader:
        yield one_line
        
  except Exception as e:
    # 打印出异常行到终端
    print(str(e))
    print(one_line)
```

函数 `get_csv_data` 在遍历数据时，使用了 `yield` 关键字，而不是 `return`，我们构建了一个[`生成器`](https://www.liaoxuefeng.com/wiki/1016959663602400/1017318207388128)

所以，并不会将所有的数据一次性读取到内存当中

只有当我们使用`next(get_csv_data())` 或者 `for each_row in get_csv_data()` 时，csv 文件中的每一行数据才会被读取到内存中

好了，我们还有一个问题没有解决，数据的一一对应

## 0x02 如何保证数据，行行对应？🤔

这里可以使用 `zip` 函数
它能够像【拉链】一样，将两个列表的数据，一一结对，组成一个元组

我们分别读取两份文件，构建两个 `csv` 文件的生产器
借助`zip` 函数，验证数据是否是同时读取

```python
A_file = get_csv_data("A.csv")
B_file = get_csv_data("B.csv")

for a, b in zip(A_file, B_file):
  print("A_file: ", a[1])
  print("B_file: ", b[1])
  
# 输出结果
A_file: 100
B_file: 90
A_file: 100
B_file: 90
A_file: 100
B_file: 90
```

> 如果我们用两个 `for` 循环嵌套的思路，会出现重复遍历的问题


## 0x03 最终代码✨

文件读取、行数一一对应的问题解决了

最终代码如下

```python
imoprt csv

# 忽略 csv 的表头，直接从数据行开始读取
A_file = get_csv_data("A.csv", ignore_header=True)
B_file = get_csv_data("B.csv", ignore_header=True)

# 如果两份文件的行数不一致，会以行数最少的 csv 文件为准
# 列表推导式
result = sum([int(a[1]) - int(b[1])] for a, b in zip(A_file, B_file))

# 假设只有三行，最终结果会是
>>> print(result)
>>> 30
```

## 0xFF 其他思路💭

1. pandas 读取 csv 数据，使用 datafram 对象的取值特性，可以直接相减，但是空间占用问题待验证；
2. 分割文件, 使用多线程同时处理，将多个线程的数据汇总；
