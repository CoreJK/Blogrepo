---
title: "让python去翻“垃圾”——遍历文件夹"
date: 2019-07-22T16:05:35+08:00
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

## 概述
处理电脑的日常事务，免不了要和文件夹（或者说工作目录）打交道
好在python提供的内置模块：os，提供许多便利的操作
今天学习了该模块内的 os.walk() 方法，用于遍历所选文件夹内所有子文件夹，
以及，每一个文件夹内所包含的文件。

## 函数长这样
```
#!python3
os.walk(<目录>)
```
-  返回值：该方法没有返回值
- 读取目录下的每一个文件夹(包含它自己)
产生3-元组 (dirpath, dirnames, filenames)【文件夹路径, 文件夹名字, 文件名】
**所以，可以用 for 循环，将元组解包，用于后续的遍历操作**

## 实例
![目标文件夹.png](https://ae01.alicdn.com/kf/Ud5fa4f917fbf48d4a0f0b8702b71448bh.png)

## 代码
```
#!python3
#walk.py - 用于遍历文件夹和子文件夹
import os
def walk_floders(folder=str(os.getcwd())): #默认获取脚本所在文件夹，传入os.walk()
	for folderName, subfolders, filenames in os.walk(folder):
		print('当前目录：' + folderName)

		for subfolder in subfolders:
			print('子目录' + ' `-- ' + subfolder)

		for filename in filenames:
			print('当前目录包含文件 ' + os.path.basename(folderName) + ' <-- ' + filename)

		print('')

def main():
    print(os.getcwd())          #输出脚本目前工作的目录
	if len(sys.argv) == 2:      #如果指定了读取的目录，传入目标目录
		tar_floder = sys.argv[1]
		walk_floders(tar_floder) 
	else:                          #否则，获取脚本所在目录，遍历
		walk_floders(tar_floder)

if __name__ == '__main__':
	main()
```
## 运行
- 我写了一个 run.bat 可以方便启动脚本
**将 run.bat 脚本放置到和要运行的脚本于同一目录下（要记得把放 run.bat 的脚本的文件夹，添加到系统环境变量中去）
或者，如下，指定要运行脚本的放置路径**
```
@python3.exe D:\py\Auto\walk.py %*
@pause
```

- 我导入了 sys 模块，让脚本从命令行获取我选择遍历的目录
- “win + R” 启动如下窗口，输入需要遍历的目录。
![运行.png](https://ae01.alicdn.com/kf/U41028c35115d4f0f88be562eef9cf0f1a.png)

## 效果
![运行结果.png](https://ae01.alicdn.com/kf/U57edac645b7549858f94085a2a405cfd1.png)

## 总结
有时候，我们要循环搜索某文件夹，及其子文件夹下的所有特定格式的文件（例如.txt, .jpg, .pdf 等）
用**os.walk()**方法，配合** for 循环，正则表达式匹配，os, shutil等文件处理模块
可以轻松处理大量的**移动，查找，改名，删除，替换**等费时费力的工作了

**人生苦短，我用python**