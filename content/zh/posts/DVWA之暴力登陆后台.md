---
title: "DVWA之暴力登陆后台"
date: 2019-04-13T22:38:16+08:00
description: "DVWA靶场通关"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "OWASP-TOP 10"
- "DVWA靶场"
categories:
- "渗透测试"
series:
- "技术研究"
image: 
---

- 漏洞类型：弱口令登陆后台

- 安全级别：Low

- 目标：通过口令字典，登陆网站后台

## 前言

现在，假设我们通过扫描网站后台，找到了后台登陆页面
![后台登陆页面](https://ae01.alicdn.com/kf/U32329d12329f4d4cb969bf895f04e5d8f.jpg)

但是，我们不知道后台管理员得账号和密码
查看DVWA提供的登陆页面php源码
![页面源码](https://ae01.alicdn.com/kf/U2209547af7f7455d80d44dc790651410p.jpg)

## 思路
**1. SQL注入**
**2. 暴力猜解用户名/密码**

## 一、SQL注入
**服务器没有对user和password进行过滤处理，可能存在sql注入：**<br>
username: admin' and 1=1 -- -<br>
password: 123<br>

成功
![成功](https://ae01.alicdn.com/kf/Uaa3db91c2e70428199f4e474ba3257c1m.jpg)

## 二、暴力猜解
### 流程图
![流程图](https://ae01.alicdn.com/kf/U0e0c1087a36e4b829e045dd0f079575b6.jpg)

## 1. Burp抓包
![Burpsuit端口及抓取目标筛选](https://ae01.alicdn.com/kf/U5e2a09d5433f4490be78442432977211N.jpg)


![抓包成功](https://ae01.alicdn.com/kf/Uf4c60dda0433403eac670cb23bd2c1b0c.jpg)

## 2. 将报文发送至Intruder
![将报文发送至Intruder模块](https://ae01.alicdn.com/kf/U6f9b1d6a76f64c4a9cb68b7399bb0d318.jpg)

## 3. 标记暴力猜解的值
先点击“Clear§”清除默认标记<br>
再用鼠标标记报文中username，password后的值，选择"Add§"<br>
![请求体中选择用户和密码](https://ae01.alicdn.com/kf/Uff9f77c4b57e41e6b5f33ede2e6195d5c.jpg)


## 4. 选择暴力猜解
![选择攻击方式](https://ae01.alicdn.com/kf/U69d3fcfc5dee443c88a311d33e8e08ca6.jpg)

## 5. 选择攻击字典
由于是测试，我们已经预知正确的用户名（admin）和密码（password）<br>
所以字典我们手动输入几个无关项即可<br>
![可能的用户名](https://ae01.alicdn.com/kf/U7aebffb93ada49b294b96bde80e28718Q.png)

![可能的有效密码](https://ae01.alicdn.com/kf/U6133343d86474bb99b824beb9617950dX.png)
通过排列组合，我们获得了12种可能的用户名和密码的组合，用于测试。

## 6. 开始测试

设置线程（默认5）
![其他选项](https://ae01.alicdn.com/kf/Ucaed0399eb164290af18c32105947579e.jpg)
开始测试
![开始测试](https://ae01.alicdn.com/kf/Uebe527f930114f36829ebb686d87a024T.jpg)

## 7. 成功/尝试登陆
测试结束，可以看到成功和失败，返回的页面中的字节长度是有差异的<br>
Lenght中可以看到正确的组合，所获得的页面返回值不同与其他组合（或长或短）<br>
初步判定，我们找到了正确的用户名和密码<br>
![返回结果](https://ae01.alicdn.com/kf/Ud7fa992d314b405da2f47983dd4e83c9s.png)

![对比.png](https://ae01.alicdn.com/kf/U989f75c852ea4290a674624b665341b67.png)

回到后台登陆页面，输入尝试
![验证](https://ae01.alicdn.com/kf/U5a73109aeabe49fd9e78d9b4c0a3f7far.jpg)
成功！
![成功登陆后台](https://ae01.alicdn.com/kf/Ua2a5af03f6924c87831b494c9c34e3150.png)

## 心得
这里只是熟悉了暴力破解的过程<br>
真实的情况下，如果不知道可能有效的用户名<br>
是很难通过暴力猜解来登陆后台的<br>
需要花时间通过社工等方式，获取足够多的信息<br>
才能事倍功半，减少不必要的等待时间<br>
或者，另辟蹊径（碰运气）<br>
没准有SQL注入漏洞呢~