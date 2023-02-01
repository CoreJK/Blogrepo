---
title: "Python用json模块读取json文件乱序"
date: 2023-02-01T22:47:57+08:00
description: "当我们用 json.loads 读取了一份 Json 文件中的数据
修改了某些键的值，再使用 json.dumps  将修改以后的数据，存入新的 json 文件时（序列化）
可能会遇到所有的 key 值乱序的情况，怎么办呢？"
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

## 问题场景

- python 版本： 3.6
- 操作系统： windows / linux

当我们用 `json.loads` 读取了一份 Json 文件中的数据（反序列化，转换为 python 可以处理的数据类型）

修改了某些键的值，再使用 `json.dumps`  将修改以后的数据，存入新的 json 文件时（序列化）

可能会遇到所有的 key 值乱序的情况，如下所示

1. 修改前
```json
{
	"key1": "value1",
	"key2": "value2",
}
```

2. 修改完成， json.dumps 重新写入文件中，出现乱序
```json
{
	"key2": "modify",
	"key1": "value1"
}
```

## 解决方案

为了确保将修改以后的数据，在序列化写入新的 json 文件时，不会出现乱序的情况

我们需要使用 `json.loads`  提供的 [`object_paris_hook`](https://docs.python.org/3/library/json.html?highlight=json%20loads#json.loads] "`object_paris_hook`")可选参数

在使用 json.loads 反序列化为 python 中的`数据类型`时候

结合 collections 模块的 OrderedDict 方法，使得反序列化，得到的有序的字典对象

确保 key 在修改完成以后不会乱序

```python
#!/usr/bin/python3

import json
import collections

# 用于修改 json 数据的内容的函数
def modify_js_file(data_dict: str, target_key, target_value) -> dict:
	"""修改js文件中任意 key 的 值"""
	pass

# 读取 json_file.json 文件中的内容
with open("json_file.json") as js_file:
	# 给 object_hook 参数，传入 collections.OrderdDict 方法
	data_dict = json.loads(js_file.read(), object_pairs_hook=collections.OrderedDict)
	# 执行修改数据内容的函数
	result = modify_js_value(data_dict, "key1", "new_value")
	# 修改后的内容，写入 new_json.json 文件中
	with open("new_json.json", 'w') as new_js_file:
		# 缩进为 4 个空格, 允许中文字符
		new_js.write(json.dumps(result), indent=4, ensure_ascii=False)
		print("修改完成")
```

## json.loads 中有趣的参数

在解决这个问题的时候，还发现一个有趣的参数
`json.dumps` 的 `seprators`
下一篇笔记再详细分享~
