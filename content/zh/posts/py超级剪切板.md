---
title: "超级剪切板"
date: 2019-07-22T16:03:33+08:00
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

# 概述
**电脑的剪切板，只能复制一样内容，然后粘贴，然后再去复制其他内容，再粘贴**
如果只是复制少量有限次的内容，在 wps 和浏览器之间直用切换一两次，倒无所谓
**但如果要从 Excel 表格中复制大量的内容，内容而且还要分门别类，复制到 新的Excel 表格中去... ...**
(╯‵□′)╯︵┻━┻

所以，让python来帮我，一次记住所有复制的内容，用助记符分类，
然后通过用户输入的命令，把复制好的内容，按需要填入 Execel 表格中去

# 用到的模块
- pyperclip 可以复制剪切板的内容为脚本所用，也可把变量值，复制到剪切板，供人类使用
- shelve 将含有值的变量，保存脚本之外，会生成三个文件【<.mcb>, <.dir>,< .bak>】
- sys 用于获取用户输入的选项（options）
- os 配合.bat文件使用

# 实例
如图，有学号，名字，电话... ...
你的任务就是，从中复制回家人员的信息，放置到新的 Excel 表格中去
![表格.png](https://ae01.alicdn.com/kf/U64297a550def40759c256a16468b06d9z.png)

# 代码
```python
#-*- coding:utf-8 -*-
#!python3 
#mcb.py - 把剪切板的内容，临时存储到二进制文件里这里，供随时调用
#Usage: py.exe mcb.pyw save <keyword> - 保存剪切板的内容，用关键词标记
#		py.exe mcb.pyw <keyword> - 根据关键词，把对应内容复制到剪切板
#		py.exe mcb.pyw list - 列出所有内容的关键词，到剪切板 

import sys, os
import pyperclip
import shelve

os.chdir(r"D:py\Auto\mcb")
mcbShelf = shelve.open(r'.\mcb')

#保存剪切板的内容，用助记符标记
if len(sys.argv) == 3 and sys.argv[1].lower() == 'save':
	mcbShelf[sys.argv[2]] = pyperclip.paste()
	print("[*] " + sys.argv[2] + "已保存")
#清空指定内容
elif len(sys.argv) == 3 and sys.argv[1].lower() == 'delete':
	del mcbShelf[sys.argv[2]]
	print("[!] " + sys.argv[2] + "已删除")


elif len(sys.argv) == 2:
      #列出关键词和加载内容
	if sys.argv[1].lower() == 'list':
		pyperclip.copy(str(list(mcbShelf.keys())))
		print("[*] 关键词已复制到剪切板！")
     #清空所有内容
	elif sys.argv[1].lower() == "delete":
		mcbShelf.clear()
		print("[!] 已清空")
    #根据关键词，把内容复制到剪切板以供粘贴使用
	elif sys.argv[1] in mcbShelf:
		pyperclip.copy(mcbShelf[sys.argv[1]])
		print("[*] " + sys.argv[1] + " 内容已加载到剪切板")

else:
	print("[!] 参数错误！")
mcbShelf.close()
```
# <mcb.bat> 文件
```bat
@python3.exe D:\py\Auto\mcb\mcb.py %*
@pause
```

# 使用方法
- **mcb save <关键词（暂不支持中文）>**
**我先复制了一行信息，win + R 输入指令
按照名字，依次保存复制的内容**
![保存复制到剪切板的内容.png](https://ae01.alicdn.com/kf/Ue16f8497ef4b44d7b6c5fcb318a45b5ef.png)


![提示已保存.png](https://ae01.alicdn.com/kf/U00c7ab19e0da441cb093c9d31d53f6c8O.png)

一次标记一个复制内容，然后复制下个人的信息
**再次 win + R 用新的关键词标记......**

- **mcb list**
**看看我们标记了几个内容**
![查看所有关键词.png](https://ae01.alicdn.com/kf/Uf4244024062945cead0999604085ff34p.png)

![提示已复制到剪切板，复制到运行窗口即可.png](https://ae01.alicdn.com/kf/U5241cf62914d49b89bbc1cc8aa4eac0fp.png)

**然后，选择要复制到新表格的内容**

- **mcb <关键词(暂不支持中文)>**
![加载 rwm 信息到剪切板](https://ae01.alicdn.com/kf/U1ab14a6efd31462ba9ebf0042e1885a72.png)

![内容已复制到剪切板.png](https://ae01.alicdn.com/kf/Ub4ea6353b1f64278ace12664b2917c826.png)


- **mcb delete <关键词（可选）>?**
**清空复制的所有内容**
![mcb delete.png](https://ae01.alicdn.com/kf/U2683a7ff8d2646758b45340751e5e3ddd.png)

![提示已清空所有内容.png](https://ae01.alicdn.com/kf/Udc0496f044d1466f84b72f8e8eb90457f.png)

**删除指定内容**
![删除对应内容.png](https://ae01.alicdn.com/kf/U64ac3b4af4bc467dabb179da6713efa2b.png)

![指定内容已删除.png](https://ae01.alicdn.com/kf/Uee11493a8dd242d4b0474b53617501dfB.png)


# 总结
- 数据和脚本程序分开储存，更加有利于脚本的维护
- shelve 模块，shelve的对象，可以用字典的方法，例子中关键词就是字典的 "键"，保存的内容就是“值”
- 相对更安全
- 代码应该可以用“类”重新组织一下，有空在弄

**这个脚本日常生活中挺实用的~**