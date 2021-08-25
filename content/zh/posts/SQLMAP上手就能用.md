---
title: "sqlmap上手就能用"
date: 2019-04-13T22:38:24+08:00
description: "SQL注入工具"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "sqlmap"
- "sql注入"
categories:
- "渗透测试"
series:
- "工具使用"
image: 
---

Sqlmap是用python2所写的脚本程序，用来对Web应用进行“sql注入漏洞”自动化测试
可以说是Web渗透测试**“护发”**利器之一
下面总结一下使用它，对网站测试Sql注入漏洞
最简单、常用到的指令


## 一、操作环境
- Windows10
- cmd
- python2.7

## 二、基本命令

### 1. 选择目标网址测试

```
sqlmap -u 目标网址
```

### 2. 确认存在漏洞后，爆破出数据库

```
sqlmap -u 目标网址 --dbs
```

### 3.  选择一个数据库，爆破出有多少表

```
sqlmap -u 目标网址 --tables -D 目标数据库名
```

### 4. 爆破出表中的字段信息

```
sqlmap -u 目标网址 --columns -T 目标表名 -D 目标数据库名
```

### 5. 尝试破解所选字段里的内容，并转存到本地计算机

```
sqlmap -u 目标网址 --dump -C 目标字段 -T 目标表名 -D 目标数据库名 
```

## 三、可选，但常用的指令
### 1.  随机选择“User-Agent”
某些站点会对过高频率访问的用户
识别他们的“User-Agent”，来做出限制
利用sqlmap自带的随机选择Agent参数，来应对

```
sqlmap -u 目标 --random-agent
```

### 2.  选择代理服务器，访问目标
理由同上,网站采取更加严格的防护
限制过高频率访问的IP地址，对网站的请求
要使用代理，需要自己搭建一个**代理池**

```
sqlmap -u --proxy=http://127.0.0.1:端口号
```

### 3.  多线程设置

```
sqlmap -u --threads 5
```
## 四、总结
实际操作中，经常需要用到**“--random-agent”,“--proxy”**等指令<br/>
原因是，网站通常会对高频率访问的用户，做出一些限制，确保网站能够正常访问<br/>
这时候我们就需要采用灵活的应对策略<br/>
来确保测试的正常进行。<br/>
以上这些操作，是目标站点没有WAF保护时的一些操作。<br/>
当然，sql还能做更多。<br/>
以后慢慢学习~