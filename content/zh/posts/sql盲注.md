---
title: "Sql 盲注好帮手，Burpsuit"
date: 2020-03-08T10:08:54+08:00
description: "Burpsuite 盲注出 cmd5 数据"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "Burpsuit"
- "sql注入"
categories:
- "渗透测试"
series:
- "工具使用"
image: 
---


## 环境

- windows 10
- DVWA靶场，等级 low，Get型 / 单引号盲注

如果不巧碰到的存在注入的站点，只能盲注，因此只能逐个猜解出，数据库名、表名、字段名和字段值
而我们提取目标在 dvwa 数据库中的 users 表下的 password 的值
出了一般来说，我们爆破的”库名~字段名都是26个英文字母的组合

而下面的密码是经过了 cmd5 加密的，一共有32位 

| user | password  |
| :------: | :------------: |
| admin | 5f4dcc3b5aa765d61d8327deb882cf99 |

这时候就需要借助 Burpsuite 内的 Intruder 模块
构建适合猜解 cmd5 的 payload

## 确定密码的长度

选择 Battering ram 的攻击类型，选中需要爆破的参数位置

![判断字段值的长度](https://ae01.alicdn.com/kf/U5990243b4be94276b9b6a5734c8bf1de6.jpg)

假设我们不知道有多少位，因此 payload 我选择从1~99

![选择payload](https://ae01.alicdn.com/kf/Uf783f539057c466c907a8e017c7cb0edY.jpg)

结果中可以看出，密码长度是32位的

![得到密码位数](https://ae01.alicdn.com/kf/U66f2759983d54d8187f908b44e257a8b0.jpg)



## 爆破出 password 的字段值

我们大胆猜测，密码是 cmd5 的格式

因此选择 cluster bomb 攻击类型，因为有两个参数的位置需要爆破

![爆破出password字段内的值](https://ae01.alicdn.com/kf/U1df57d1d34cf44c8b91b0954196307c7P.jpg)

第一个位置对应的返回字符串的第几位

![password_payload1](https://ae01.alicdn.com/kf/U4e01933050d4440d81521ccc3ae7226cC.jpg)

第二个对应就是猜测“字典”了
因为 cmd5 是由 a~z 和 0~9 组成的
盲注的语句，又只能一位一位去猜测，一次请求只能猜解出一个

![password_payload2](https://ae01.alicdn.com/kf/U3f062cdd8aa14c52858818d26be0b9d2l.jpg)

最终得到的结果如下图
可以看到，无法按照 payload1 请求顺序排列出 cmd5 值

应该是 payload2 既有字母，也有数字导致的
这里可以自己写脚本来对结果进行排序（还在研究中... ...）

![爆破出密码的cmd5值](https://ae01.alicdn.com/kf/U0db5b93b18654e81a4f141b962547fe1Y.jpg)

## 写在最后

这次的练习，暴露出自己对于 Burpsuite 的 Iturder 模块的生疏
但是通过查询资料和求助大佬得知
盲注在实战中，往往要采用 Bool 等语句，向服务器多次发起请求

容易被 waf 识别，ban ip地址（有自己的 ip 代理池另说）
所以，还有更好的方案来应对盲注

> - **dnslog 注入**
>
> - windows下的 UAC 路径

那就是下一个故事了 XD

