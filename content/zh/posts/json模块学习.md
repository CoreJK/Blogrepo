---
title: "Json模块学习"
date: 2023-02-05T00:06:00+08:00
description: "json.load 和 json.loads 有什么区别？json.dump 和 json.dumps 有什么区别？"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "python"
categories:
- "编程语音"
series:
- "技术研究"
image:
---

![json 转换](https://s2.loli.net/2023/02/04/txYXdebwMgVBman.png)

## 疑惑

写完了上篇笔记 《json.loads 数据后，再使用 json.jumps 出现 key 值乱序的解决方案》
以后心中产生了一个疑惑：

> json.load  和 json.loads 有什么区别？
>
> json.dump 和 json.dumps  有什么区别？

继续查阅了 `python` 官方的 `json` 模块，对于这 `json.load(s)` 和 `json.dump(s)`两个方法的解释以后

明白了两者异同只要，结合实际应用，得到他们的适用场景

## 一、json.load 与 json.loads

表面上看，一个没有尾巴 s 一个有😅

文档中，对于 `json.load` 方法是这么描述的📃

**`json.load` 方法支持参数的描述**

> json.load(fp, *, cls=None, object_hook=None, parse_float=None, parse_int=None, parse_constant=None, object_pairs_hook=None, **kw)

**对于该方法作用描述**
> Deserialize fp (a .read()-supporting text file or binary file containing a JSON document) to a Python object

从一个支持 `read()`方法的文件对象中，读取符合 json 标准定义 [RFC 7159](https://datatracker.ietf.org/doc/html/rfc7159.html "RFC 7159") 和  [ECMA-404](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/ "ECMA-404") 的内容，这些内容的存放形式，可以是文本，也可以是二进制的形式

然后将 json 字符内容，通过 `json.JSONDecoder` 解码器，一一转换为 `python` 中的数据类型，也就是下面的对应关系⬇️

|      JSON      |    Python    |
| :------------: | :----------: |
| object -- 对象 |     dict     |
|     array      | list -- 列表 |
|     string     |     str      |
|  number (int)  |     int      |
| number (real)  |    float     |
|      true      |     True     |
|     false      |    False     |
|      null      |     None     |



`json.loads`  除了第一个参数接受的对象变成了接收 `str、byte、bytes` 类型的数据类型，而不是文件对象了，其他参数与 `json.load` 是相同的

![](https://s2.loli.net/2023/02/04/dRoZtp71Imhn596.png)

> 如何使用 python3.6 想要从字节流中读取 json 数据，需要编码为 UTF-8、UTF-16 或 UTF-32 类型再


>  如果使用 python3.9 的版本，还需要注意 encoding 参数已经去除，如果 json 文件编码中，混合使用了不同编码的字符，要注意!


### ✨何时使用，看 json 数据存放在哪里✨

所以，我们可以得到两者使用场景：
1. 如果 `json` 数据在文件（文本、二进制流）中，使用 `json.load`；

   > 注意第四行，我们可以不用显示调用 json.file.read() ，json.load 已经帮我们做了这个操作

    ```python
    import json
    with open("some_file.json") as json_file:
        # json_file.read() 被隐式调用，json.load 本身会默认调用 json_file.read() 方法
        datas = json.load(json_file)
        print(datas)
    ```

2. 如果 `json` 数据已经是字符串、字节对象，使用 `json.loads`

   ```python
   import json
   with open("some_file.json") as json_file:
       # json_file.read() 显示调用
       json_content = json_file.read()
       datas = json.loads(json_content)
       print(datas)
   ```
   
   

## 二、json.dump 与 json.dumps 

我们直接来看文档描述

![](https://s2.loli.net/2023/02/04/PEyaQAgkG2hLN1b.png)

两者作用是相同的，将能符合 `json` 数据描述的对象，转换成 `json` 数据

### 两者主要区别，和使用场景

`json.dump` 比 `json.dumps` 多了一个 fp 文件对象参数

```python
import json

host_info = {
	"ip":"127.0.0.1",
	"user_name":"root",
	"passwd":"123456",
	"port":"22"
}

# json.dump
with open("settings.json", "W") as setting_file:
	# 将 host_info 字典中的内容，隐式调用 setting_file.write() 写入 settings.json 文件
	json.dump(host_info, setting_file)
    
# json.dumps
with open("settings.json", "W") as setting_file:
    # 将 host_info 字典中的内容，显示调用 setting_file.write() 写入 settings.json 文件
	dumped_content = json.dumps(host_info)
	setting_file.write(dumped_content)
```



## 写在后面

在找到一些问题的解决方案，学到了某些函数的基本使用后，需要回过头翻看[python 官方文档](https://docs.python.org/ "python 官方文档")

网上一些记录，有时候比较零散，有时候会忽略一些函数本身提供的参数

比如 `json.dump` 的 `separataors` 参数，已经解决了 json 字符中  ',' 与 ‘:’ 格式化的问题

然后自己再造一次轮子，实际上浪费了不少时间
