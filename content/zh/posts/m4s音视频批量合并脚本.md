---
title: "bilibili缓存音视频合并脚本"
date: 2020-05-29T9:40:20+08:00
description: "这一次编写脚本，总结了一些实用的经验，以及一些应该改进的习惯"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "python"
- "ffmpeg"
categories:
- "编程语言"
series:
- "技术研究"
image: 
---

## 一、前言

从B战缓存了一些视频，想把他们提取出来，放到电脑上观看<br>

没想到，B站把视频和文件分离成了音频、视频，两个文件<br>

一个是 vedio.m4s,另一个是 auoid.m4s，于是琢磨怎么把他们合并成一个 mp4 文件<br>

一番查阅后，找到了一个口碑挺不错的命令行工具——ffmpeg<br>

由于缓存的视频比较多，逐个去执行合并命令费时费力<br>

于是便有了这个 python 脚本<br>

记录一下，编写时候遇到的一些困难，以及解决办法<br>

## 二、遇到的Bug



### ① 变量名作用域 & 形参

我遇到的问题是：“如何在子函数中，访问另一个 main 函数中的变量”

解决方法很简单，在 main() 函数中，将要被其他函数访问的变量前，加上 global 就可以了

```python
def moveMedioFile(work_paths):
	#TODO: 合并好的视频,移动到初始路径下
	logging.debug("移动视频文件到目标路径path： " + path)
	for targetFile_path, video_file in work_paths:
		mp4_file_path = os.path.join(targetFile_path, video_file + '.mp4')
		try:
			if os.path.isfile(mp4_file_path):
				logging.info("正在移动: " + os.path.basename(video_file))
				shutil.move(mp4_file_path, path)
    ----snip----
def main():
	global path
    ---snip---
    
if __name__ == '__main__':
	main()
```

我在这里卡住的原因是，潜意识里面把 main() 函数里面的变量，当作了全局变量

导致后面子函数访问时出现了 Bug

**另外，形参的变量名，尽量避免与全局变量名字取得一样**

所以用函数式编程，一定要小心变量作用域的问题！



### ② 遍历指定数量的元素

我需要处理的数据如下

``` python
folders_titleNames = ['G:\\电影\\1\\64', '复仇者联盟一', 'G:\\电影\\2\\32', '复仇者联盟二', 'G:\\电影\\3\\32', '复仇者联盟三']
#经过处理，转变为下面的形式
folder_titleName = [('G:\\电影\\1\\64', '复仇者联盟一'), ('G:\\电影\\2\\32', '复仇者联盟二'), ('G:\\电影\\2\\32', '复仇者联盟三')]
```

**解决代码如下**

```python
#TODO: 处理 folders_titleName 列表，转化视频路径、视频标题为元组，方便解包用于命令的执行
#方法一
arguments = []
for i in range(len(folders_titleNames)):
	if i % 2 == 0:
		folder_titleName = tuple(folders_titleNames[i:i+2])
		arguments.append(folder_titleName)
#方法二
i = 0
arguments = []
while i <= len(folders_titleNames):
 		folder_titleName = tuple(folders_titleNames[i:i+2])
		arguments.append(folder_titleName)
        i += 2
```
其实就是列表切片的处理，一般情况下我们只是逐个访问元素

但是上面的情况，要求两个（甚至可以多个）提取，来满足实际需求


## 总结

这次编写代码的时候，用到了很多重复的变量名成，比如 “path”

因为主要是为了处理路径和文件名信息，一开始没考虑这么多

导致最后调试的时候，极其费眼... ...

所以尽可能还是遵守 PEP8 的建议，再结合自身编写习惯与风格吧。

**也许用类来组织这段代码，思路会更清晰**

**To Be continued... ...**



