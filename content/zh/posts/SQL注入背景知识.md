+++
date = "2019-08-12T13:00:54+08:00"
categories = ["渗透学习笔记"]
tags = ["SQL注入笔记"]

title = "SQL注入基础"
description = "Mysq数据库l注入"
images = []

+++

# 前言

拿着 SQLmap 一把梭，结果就是实战中常常被 ban ip.

奈何还没搞定自己的 ip 代理池... ...

所以手工注入还是要掌握的

这里只是列出了知识点，没有详尽举例

接下来的关于MySql注入学习笔记中，都会用到

# 需要了解的知识、工具

- 理解基本的 HTTP 协议

- 能看懂 HTML 

- PHP 等后端常用语言

- MySQL 数据库的结构，存在于主机的位置

- MySQL 的查询语句基本语法

- Burpsuit、HackBar 等工具的使用

  **前面三点，需要花时间积累，一点点去学习，我也在摸索中DX.**

---

# MySql 数据库

## ① MySql 的数据库放在哪里？

下图是我自己的理解

记住这个图，对写SQL查询语句有帮助

![我的理解.jpg](https://i.loli.net/2019/08/12/CgAtlT5IDmsHpQx.jpg)

## ② 数据库的结构

![结构.jpg](https://i.loli.net/2019/08/12/vZTOefXVzYqpuQr.jpg)

## ③ 需要记住的一个库，三个表（重要！）

**这是在 MySql 5.0 及以后版本中存在的，一个存放了所有信息的地方的数据库**（假定为上图的库一）

| 库名               | 表一名称 | 表二名称 | 表三名称 |
| ------------------ | -------- | -------- | -------- |
| information_schema | schemata | tables   | columns  |

- 表一：SCHEMA_NAME字段中，存放了所有数据库的库名

- 表二：TABLE_SCHEMA、TABLE_NAME两个字段，分别存放了所有数据库名、表名

- 表三：TABLE_SCHEMA,、TABLE_NAME、COLUMMNS_NAME三个字段，分别存放了数据库名、表名、以及表中的字段名

  

##  ④ MySql 注释符号

- \#
- \--
- /\*! \*/

**对于字符型注入，注释符必不可少。**

## ⑤ MySql 查询语句

**语句的构建，要根据页面的异常回显位置拼凑。**

```sql
 SELECT 要查询的字段名 FROM 库名.表名
 SELECT 要查询的字段名 FROM 库名.表名 where 已知条件的字段名=‘已知条件的值’ limit 0,1
```

> limit 0，1表示，从第一条记录开始，取一条记录，也就是取一行

因为一张表，往往存放了许多行的信息

有时候，网站显示信息的位置有限，只能显示几行信息

limit 就能发挥大作用

## ⑥ MySql注入常用函数

### Ⅰ. 普通函数

> database() 返回当前页面内容，使用的数据库名称
>
> user()  返回当前访问数据库的用户名，身份
>
> version() 返回当前数据库的版本
>
> char() 转换 ASCII 数值

### Ⅱ. 全局函数

> @@hostname 获取主机名
>
> @@datadir 获取 Mysql 的安装路径
>
> @@version_compile_os 获取主机的系统信息

**使用举例，假如确认当前页面，有三个位置显示信息**

```sql
SELECT database(),user(),version() 
SELECT @@hostname,database(),@@version_compile_os
```

还有很多，以后补充。

# 工具

## HackBar

Chrome, firefox浏览器中插件商店下载安装，f12开启。

![HackBar.png](https://i.loli.net/2019/08/12/iAKWpxLhcvDFVqX.png)

我用的Chrome，插件商店里面有三个，我用的第一个

功能比较全面，支持post、cookie等注入姿势。

## BurpSuit

必备神器，不多说啦