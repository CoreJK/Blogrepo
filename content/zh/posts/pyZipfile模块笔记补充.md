+++
date = "2019-07-22T16:01:50+08:00"
categories = ["python"]
tags = ["笔记"]


title = "zipfile模块笔记补充"
description = "《python让繁琐的工作自动化》笔记"
images = []
+++

# 问题
现在有这样一个任务要求
#### ① 要求从一个目录下，找到所有的特定后缀的文件
#### ② 并将他们，移动到一个文件夹，或者压缩
#### ③ 无论这些文件，在当前目录下那个位置

# 思路
和昨天的问题类似，但是稍有区别
**不需要压缩所有的文件夹，只需要压缩，包含有特定后缀文件的文件夹就行**
所以会比昨天的代码少一些

# 实现思路
① \#TODO: 导入sys,os, zipfile
```python
#!python3
#find_py.py - 查找当前文件夹下的.py文件,并压缩打包

import sys, os
import zipfile
```
② \#TODO: 采用函数式编程，先写好程序入口函数（main），和处理文件的函数名（find_py）
```python
def find_py():
    pass
def main()：
    pass
if __name__ == '__main__':
    main()
```
③ \#TODO: main()程序入口 --> 获取用户输入的路径，转化为绝对路径后，传入处理函数
```python
def main():
    #获取命令行输入的脚本名称，操作目录
	scripts, path_of_folder = sys.argv
	#显示当前目录
	print("当前脚本：" + scripts)
	#命令传入findPy()函数
	abs_folder = os.path.abspath(path_of_folder)
	findPy(abs_folder)
	#结束
	print("[!] 完成")

```
④ \#TODO:完善find_py()函数，测试运行
```python
def find_py():
    #TODO:检查备份文件夹中，有没有同名文件夹，有的话N+1
	number = 1
	while True:
		backFileName =  "back_py_" + str(number) + ".zip"
		if not os.path.exists(backFileName):
			break
		number += 1

	#TODO:打开一个压缩文件
	with zipfile.ZipFile(backFileName,'w') as backupFile:
		#TODO:遍历文件夹，找到所有后缀为.py的文件，添加到压缩文件中
		for foldername, subfolder, filename in os.walk(folder):
			print("正在搜寻/压缩.py文件 %s ..." % (foldername)) #添加文件夹到压缩文件，纯粹添加文件夹s
			for pyFile in filename:
				if pyFile.endswith('.py'):
					backupFile.write(os.path.join(foldername, pyFile)) #添加文件，要添加其所在路径

```
整理
```python
#!python3
#find_py.py - 查找当前文件夹下的.py文件,并压缩打包

import sys, os
import zipfile

def findPy(folder):
	#TODO:检查备份文件夹中，有没有同名文件夹，有的话N+1
	number = 1
	while True:
		backFileName =  "back_py_" + str(number) + ".zip"
		if not os.path.exists(backFileName):
			break
		number += 1

	#TODO:打开一个压缩文件
	with zipfile.ZipFile(backFileName,'w') as backupFile:
		#TODO:遍历文件夹，找到所有后缀为.py的文件，添加到压缩文件中
		for foldername, subfolder, filename in os.walk(folder):
			print("正在搜寻/压缩.py文件 %s ..." % (foldername)) #添加文件夹到压缩文件，纯粹添加文件夹
			for pyFile in filename:
				if pyFile.endswith('.py'):
					backupFile.write(os.path.join(foldername, pyFile)) 
	return None

def main():

	#TODO:获取命令行输入的脚本名称，操作目录
	scripts, path_of_folder = sys.argv
	#TODO:显示当前目录
	print("当前脚本：" + scripts)
	#TODO:路径转化为绝对路径后，传入findPy()函数
	abs_folder = os.path.abspath(path_of_folder)
	findPy(abs_folder)
	print("[!] 完成")

if __name__ == '__main__':
	main()
```
# 总结
**写入文件时，一定要记得带上它的路径**