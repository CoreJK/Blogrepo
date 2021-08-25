---
title: "Git-Bash安装tree命令"
date: 2019-02-24T11:35:10+08:00
description: "把tree命令，安装到Git-Bash中"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- ""
categories:
- "Git"
series:
- "工具使用"

image: 
---

## 折腾起因
初次使用Git-Bash，想要使用tree命令来查看目录结构。
没料到在CMD中预置的tree命令，Git-Bash中没有预置

## 解决思路
**CMD中报错**
```
xx 不是内部或外部命令，也不是可运行的程序或批处理文件。
```

**Git-Bash中报错**
```bash
bash: xx: command not found
```
和解决CMD问题一样的思路

1. 通过互联网找到缺失文件
2. 下载缺失文件后，放到Git安装目录下的bin文件中
3. 运行Git-Bash测试是否安装成功

## 安装tree.exe文件
[*tree命令的二进制zip包*](https://jaist.dl.sourceforge.net/project/gnuwin32/tree/1.5.2.2/tree-1.5.2.2-bin.zip)

- 解压zip文件，找到解压文件中的bin文件夹下的tree.exe文件
- 复制到Git安装目录下的bin文件夹中

我把Git安装在E盘中了

```
E:\\Git\usr\bin
```

如果安装没有修改路径安装了Git，默认是

```
C:\\Program Files\Git\usr\bin
```

## 最后一步
完成以上步骤后，在Git-Bash中运行一下指令

```bash
tree --version
```

显示版本信息，说明安装成功

```
tree v1.5.2.2 (c) 1996 - 2009 by Steve Baker, Thomas Moore, Francesc Rocher, Kyosuke Tokoro
```

tree有如下可选参数

```
  -a 	列出所有文件。
  -d 	仅列出目录。
  -l 	遵循目录等符号链接。
  -f 	打印每个文件的完整路径前缀。
  -i 	不要打印缩进线。
  -q 	将不可打印的字符打印为“？”。
  -N 	按原样打印不可打印的字符。
  -p 	打印每个文件的保护。
  -u 	显示文件所有者或UID号。
  -g 	显示文件组所有者或GID编号。
  -s 	打印每个文件的大小（以字节为单位）。
  -h 	以更易读的方式打印尺寸。
  -D 	打印上次修改的日期。
  -F 	附加'/'，'='，'*'或'|'按照ls -F。
  -v 	按版本按字母数字排序文件。
  -r 	按反向字母数字顺序对文件进行排序。
  -t 	按上次修改时间对文件进行排序。
  -x 	仅保留当前文件系统。
  -L 	level只降低级别目录深度。
  -A 	打印ANSI线条图形缩进线。
  -S 	使用ASCII图形缩进线打印。
  -n 	始终关闭着色（-C覆盖）。
  -C 	始终打开着色。
  -P 	pattern仅列出与给定模式匹配的文件。
  -I 	pattern不列出与给定模式匹配的文件。
  -H 	baseHREF使用baseHREF作为顶级目录打印出HTML格式。
  -T 	string用字符串替换默认的HTML标题和H1标题。
  -R 	当达到最大目录级别时重新运行树。
  -o 	file输出到文件而不是stdout。

  --inodes 	打印每个文件的inode编号。
  --device 	打印每个文件所属的设备ID号。
  --noreport 	在树列表末尾关闭文件/目录计数。
  --nolinks 	关闭HTML输出中的超链接。
  --dirsfirst 	列出文件前的目录。
  --charset 	X使用charset X进行HTML和缩进行输出。
  --filelimit   ＃不要在其中包含多于＃个文件的dirs。
```
愉快的使用tree命令吧~(雾)