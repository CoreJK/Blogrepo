---
title: "将文本数据分组，并一次放入不同文件"
date: 2019-07-27T20:42:53+08:00
description: "python处理文本数据"
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

## 场景
现在手头有这样的一个任务：

- 一个文本文件内，每一行有一串 16进制 数字，一共有577行；
- 某程序会用到这些16进制数字，但是只能一次从一个文本文件中，读取100行，才能保证稳定和效率；
- 保证数据没有重复，并且按照降序排列；
- 所以，我们需要把这些数字，分组放置到多个个文件中去，这样程序才能正确的处理；

**要处理的577行数据**

```
ffffffffffff
000000000000
a0a1a2a3a4a5
b0b1b2b3b4b5
c0c1c2c3c4c5
d0d1d2d3d4d5
--- skip ---
d3f7d3f7d3f7
5a1b85fce20a
714c5c886e97
587ee5f9350f
... ...
```



---


## 思路
也就是说，每一个新的文本文件内，只含有100行数据
接下来，我们需要让 python 脚本作如下的事情

1. 打开要处理的文件，先把内容用 readlines()，数据被组织成列表，去重排序；
2. 创建新的文件,用到 open("file", 'w')；
3. 文件名称的最后，加上不同的数字，按顺序增加；
4. 每创建一个新文件，写入100行；然后把在列表中，删除掉已经写入的部分；

---

## 代码实例
### ① 将要处理的数据，读取出来；

```python
#!python3
with open("stds.keys") as file: 
	all_keys = file.readlines()
```

### ② 根据任务需要，我要先去除重复数据,降序排列，再交给接下来的函数去写入不同的新文件；

```python
tmp = list(sorted(set(all_keys), reverse=True))
>>> 交给写好的 separate() 函数去处理，
separate(tmp, 100)                                           
```

### ③ 函数中的代码，会根据列表的中的数据多少，每个文件写入多少数据，生成合理新文件数量；

```python
>>> 这里我用了logging模块来调试
logging.basicConfig(level=logging.DEBUG, format="%(asctime)s - [%(levelname)s] - %(message)s")
#logging.disable(logging.CRITICAL)
def separate(key_list, write_key_num):
	logging.info("开始处理")
	logging.debug("密钥数量： %d " % (len(key_list)))

	>>>  根据密钥总数 / 每个文件写入的行数，向上取整，得到生成文件的数量 num
    >>> 现在 num = 6
	num = math.ceil(len((key_list)) / write_key_num)
    logging.debug("生成文件数量: %d" % (num))
	for fileNum in range(num):
		with open("extend_%s.keys" % (fileNum + 1), 'w') as key_file:
			key_file.write("#keys\n")
			
			for key in key_list[:99]:
				key_file.write(key)

			try:
				del key_list[:99]
				logging.debug("剩余 %d 密钥未写入文件", len(key_list))
			except Exception as e:
				raise e
```

**这里需要注意的是，range(num) 会生成 0~5 的数字
所以，会依次生成 6 个新文件，为了保证文件名的不同
每个新文件名字里，都加入了一个序号**

> with open("extend_%s.keys" % (fileNum + 1), 'w') as key_file:

**要记得给 fileNum + 1 ，不然文件名的后缀，就会从 0 开始 ，一直到 5，而我们需要的是 6.**

## 最后整理&效果

```python
#!python3
#filter.py - 把文件中的内容分割，然后分别保存到几个文件中

import logging
import math
logging.basicConfig(level=logging.DEBUG, format="%(asctime)s - [%(levelname)s] - %(message)s")
#logging.disable(logging.CRITICAL)

def separate(key_list, write_key_num):
	logging.info("开始处理")
	logging.debug("密钥数量： %d " % (len(key_list)))
	
	num = math.ceil(len((key_list)) / write_key_num)
	logging.debug("生成文件数量: %d" % (num))
	for fileNum in range(num):
		with open("extend_%s.keys" % (fileNum + 1), 'w') as key_file:
			key_file.write("#keys\n")

			for key in key_list[:99]:
				key_file.write(key)
			try:
				del key_list[:99]
				logging.debug("剩余 %d 密钥未写入文件", len(key_list))
			except Exception as e:
				raise e
			
	logging.info("Done!")
def main():
	#打开要处理的文件
	with open("stds.keys") as file:
		#到 all_keys 
		all_keys = file.readlines()
	tmp = list(sorted(set(all_keys), reverse=True))

	#交给函数处理，根据密钥数量，保存到多个文件中
	separate(tmp, 100)
	print("处理完成！")

if __name__ == '__main__':
	main()
```

![运行调试](https://ae01.alicdn.com/kf/U496ae26efa2c40aab8a59c566176be3cO.png)

**好了，数据组织好了。**
最然真实的数据不会这么整齐，简单；
我主要的总结的方法，是如何一次生成多个文件，并且保证文件名有序
也许还可以把文件名，放到一个列表中，然后用 random 模块，随机命名
在某些场景下也许用的到。