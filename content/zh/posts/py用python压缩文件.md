+++
date = "2019-07-22T16:05:02+08:00"
categories = ["python"]
tags = ["笔记"]


title = "用python压缩文件"
description = "《python让繁琐的工作自动化》笔记"
images = []
+++

![压缩文件ing.png](https://ae01.alicdn.com/kf/U7f9fe7021eae44ce9617648316d6dd206.png)

# 概述
上次利用 **python** 中的 **os** 模块的**<os.walk()>**成功遍历的目录树
昨天又学到 **<zipfile>** 模块来压缩文件
那么，两者结合起来，就能实现简单的压缩、打包文件的功能了。

# 程序目的
每执行一次，新建一个压缩文件，压缩文件名如下
- <备份目录名>_N.zip
- 下一次执行脚本文件，数字 N+1 
备份的zip文件，放入back_up 文件夹中

# 代码实现思路
① \#TODO: 导入 zipfile, os 模块
```
#!python3
import os
import zipfile
```
② \#TODO: 检测 back_up 目录下，是否存在同名压缩文件，存在则生成n+1的新文件名
```
number = 1
while True:
	zipFilename = os.path.basename(folder) + '_' + str(number) + '.zip'
	if not os.path.exists(zipFilename):
		break
	number = number + 1
```
③ \#TODO: 以 'w' 新建一个压缩文件，用 ② 的 zipFilename 命名
```
with zipfile.ZipFile(zipFilename, 'w') as backZip:
```
④ \#TODO: 接下来，遍历要压缩的目录，把每一个目录，和其下的文件都写入压缩文件
```
#os.walk()遍历每一个目录下的文件
    for folderName, subfolders, filenames in os.walk(folder):
	    print('当前目录下文件正在压缩 %s ...' % (folderName))
	    #先添加文件夹，到备份zip压缩包中去
	    backupZip.write(folderName)
	    #再添加当前目录下的文件到压缩包中去
	    for filename in filenames:
		    backupZip.write(os.path.join(folderName, filename))
```
⑤ \#TODO: 提示完成
```
print("[!] 备份完成.")
```
# 整理代码
```
#!python3
#backupToZip.py - 备份任意目录下所有文件，到 back_up 文件夹中

import sys, os, time
import zipfile


def backupToZip(folder):
	#备份输入的目录下的所有内容
	folder = os.path.abspath(folder) #输入的路径转化为绝对路径
	number = 1
	while True:
		zipFilename = os.path.basename(folder) + '_' + str(number) + '.zip'
		if not os.path.exists(zipFilename):
			break
		number = number + 1

	print('建立备份文件 %s ...' % (zipFilename))
	with zipfile.ZipFile(zipFilename, 'w') as backupZip: 
		for folderName, subfolders, filenames in os.walk(folder):
			print('当前目录下文件正在压缩 %s ...' % (folderName))
			backupZip.write(folderName)
			
            for filename in filenames:
				backupZip.write(os.path.join(folderName, filename))
	print('[!] 备份完成.')

def main():
	os.mkdir(r'.\back_up')
    os.chdir(r'.\back_up')
	print('backupTozip脚本运行目录：' + str(os.getcwd()))
    # 选择要备份的文件夹，以及所在的路径
	target = r'D:\py\Auto\delicious'
	backupToZip(target)

if __name__ == '__main__':
	main()
```

# 总结
- 和文件有关的操作，要记得检查**路径、文件夹或文件**是否存在、是否有**权限（r,w,x）**等
下面这段，通常用来判断
**文件不存在、没有权限等情况，执行 if 内的代码，要么报错，要么放心的去做其他的事情**
```
if not os的<文件夹、文件>检查方法:
    pass
```
- 检查文件，是否以 "xxx" 名字开始，并且以某种后缀结尾
**下面这段，检查文件是否为 xxx.zip 文件** 
```
if filename.startswith("xxx") and filename.endswith('.zip'):
    pass
```
