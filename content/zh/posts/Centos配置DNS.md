+++
date = "2019-04-28T08:05:51+08:00"
categories = ["Linux"]
tags = ["centoos", "网络"]


title = "Centos配置DNS"
description = "linux网络配置问题"
+++

# 前言
折腾vps的时候，遇到了网路配置方面的问题。

参考下方链接里大佬的博客里的方法，得以解决

老左博客](https://www.laozuo.org/6647.html)

# 环境
centos

# 两种方法
第一种是临时性的，重启会失效，又要手动设置；
<br/>
第二种是永久性的办法。<br>
两种方法修改的文件，所在文件路径如下<br>

方法一修改文件路径

> /etc/

方法二修改文件路径

> /etc/sysconfig/network-scripts/

## 一、临时修改当前DNS
用vim打开resolv.conf文件，文件最后添加内容如图所示：

> vim /etc/resolv.conf

![修改resolv文件](https://gitee.com/Y4er/static/raw/master/2019/08/04/sixpEMEpQUjW2zM1.jpg)

ok，**先按ESC，然后:wq，保存退出**，reboot，等待重启。
<br>
我遇到的问题是，重启后又恢复到了系统默认的状态
<br/>
**需要重新设置，比较麻烦。**

## 二、永久修改DNS方法
按照上面的方法解决不了安装软件时，提示的报错问题时，就要用最后的办法了

> vim /etc/sysconfig/network-scripts/ifcfg-eth0

![修改ifcfg-etho0配置文件](https://gitee.com/Y4er/static/raw/master/2019/08/04/S5072ALVh7v8B3O6.jpg)

如上图，添加红框内容

>DNS=8.8.8.8
<br/>DNS=8.8.4.4

保存退出，reboot重启机器。

# 写在后面

不同liunx发行系统修改的文件内容有所差异<br>
后面遇到了，再做整理<br>
DNS问题，这次是在使用yun安装软件时遇到的。