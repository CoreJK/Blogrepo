+++
date = "2019-04-13T22:38:04+08:00"
menu = ""
categories = ["渗透学习笔记"]
tags = ["DVWA靶场通关"]

banner = "banners/linux.jpg"
title = "window10部署DVWA靶场"
description = "靶场搭建"
images = []
+++

# 前言
学习web渗透测试，必须要实践，像我这种初学者尤为必要。<br/>
正好Github上就有DVWA这样一个开源项目，供Web开发者了解常见安全问题。同时也为Web渗透测试初学者，提供了便利、合法的学习环境。<br/>
**DVWA官网：[http://www.dvwa.co.uk/](http://www.dvwa.co.uk/)**<br/>
找到Download，下载得到一个压缩包。<br/>

部署DVAW并不麻烦，只要有phpstudy+DVWA，就可以搭建好目标。<br/>
下面分享我的部署经验，供大家参考，不足之处欢迎留言讨论~<br/>
然后就可以愉快的开始学习Web渗透测试中的骚操作了~（踩了一天的坑)<br/>

**PS：我默认读这篇文章的朋友，有能力搜索和安装Burpsuit，phpstudy，SwitchHosts，安装软件问题不再赘述**

## 一、环境
- 系统：win10 64位
- 浏览器：Chrome（版本73.x）
- 目标服务器：phpstudy（官网最新）
- 测试环境：DVWA（1.10）
- 修改Hosts工具：Switch Hosts

## 二、步骤
### 1. 目标服务器及DVWA站点配置

#### 1.1将DVWA放入指定文件夹
**PS：解压缩得到的文件夹名字时“DVWA-master”，要把-master删掉**
![DVWA放置路径.jpg](https://ae01.alicdn.com/kf/U9f0f6ebf4811463c912b2effbfda00b8B.jpg)

#### 1.2修改DVWA/config目录下的config文件为php
![去除文件后缀.jpg](https://ae01.alicdn.com/kf/U5f2d6f402c7b43b3a02f169323655a4eB.jpg)

- **去除php后面多余的后缀即可。**

#### 1.3修改config文件参数

- **如图修改**
![修改config参数.jpg](https://ae01.alicdn.com/kf/U7c4053d519c241b181727a73df9c72779.jpg)

#### 1.4修改phpstudy的php开关参数

- **把参数开关列表中，最后三个全部勾上**
![phpstudy里的php开关设置.jpg](https://ae01.alicdn.com/kf/U5b2a3fb007ed40e0bf838b5443711351A.jpg)

#### 1.5站点域名设置

- **点击其他选项菜单，找到站点域名管理。**
把网站域名改为
>www.dvwa.com

- **这里随意改名，开心就好。**
- **网站目录选择：phpstudy\PHPTutorial\WWW\DVWA**
- **二级域名可以留空**
- **端口80**
![php站点域名管理.jpg](https://ae01.alicdn.com/kf/Ucd6b144407e24265bb29df511b1d0467q.jpg)

最后别忘了点

- **“新增”**
- **“保存设置并生成文件”**

#### 1.6设置系统hosts
打开SwitchHosts，末尾输入

> 127.0.0.1 www.dvwa.com


这样你在浏览器输入域名，就会打开DVWA的网站了
![设置hosts.jpg](https://ae01.alicdn.com/kf/U2c53a41b3b5446acb4cf57475c3f35afT.jpg)
第一次使用SwirtchHosts，可能会遇到没有修改Hosts权限问题
请参考解决方案：
[https://jingyan.baidu.com/article/624e7459b194f134e8ba5a8e.html](https://jingyan.baidu.com/article/624e7459b194f134e8ba5a8e.html)


### 2. 系统代理/端口配置
#### 2.1打开系统代理端口
- **开始菜单——>设置——>网络和Internet——>代理**
如图所示，把**localhosts;127.\*;**删掉，这样你才能抓包。
**不要勾选“请勿将代理服务器...”**
**记得保存！！！！**

![系统代理及端口设置.jpg](https://ae01.alicdn.com/kf/U9cbdcb79d282483a9a18dcdbb2190ee97.jpg)
**保存这一步可以在你准备抓包的时候再执行。**

#### 2.2 我们现在可以先不保存，测试一下目标站点能不能访问
- **先启动Phpstudy**
- **然后在浏览器输入www.dvwa.com/DVWA/setup.php**

![DVWA数据库设置](https://ae01.alicdn.com/kf/Ua484f82d62054772920402dd20abcc88Q.png)

- **创建数据库，然后会自动跳转到登陆页面**

![测试DVWA能否访问.jpg](https://ae01.alicdn.com/kf/U91fbdfe348a0428db7f0a373864ae533u.jpg)

- **用户名admin；密码：password。进入如下页面**

![修改安全等级.jpg](https://ae01.alicdn.com/kf/U0cd717f8a4cd4e008b98ecedde00a760r.jpg)

- 修改安全等级为**Low**
![修改难度等级2.jpg](https://ae01.alicdn.com/kf/U00412ce15ad946649b718fef8e300edfa.jpg)

- **Submit提交**
到这里，DVWA已经在你电脑里部署好，并且可以访问了。
接下来就是抓包测试了

### 3. BurpSuit基本配置
![Burpsuit端口及抓取目标筛选.jpg](https://ae01.alicdn.com/kf/U5e2a09d5433f4490be78442432977211N.jpg)
- **打开系统代理，启动phpstudy软件**
1.**设置好Burpsuit监听端口**
2.**选择抓取的目标网站域名**

### 4. 抓包测试
终于最后一步了~
选择第一关，后台登陆用户名、密码爆破
![选择第一关并测试能否抓包.jpg](https://ae01.alicdn.com/kf/U4b0d80d8bd424154b5da3d4b7ce0b8aca.jpg)

可以看到，输入用户名和密码后，Burp成功截获报文。<br/>
利用这段请求报文，我们就可以利用Burp中的“Intruder”进行暴力登陆尝试，来获得正确的后台登陆的用户名和密码了
![抓包成功.jpg](https://ae01.alicdn.com/kf/Uf4c60dda0433403eac670cb23bd2c1b0c.jpg)

## 三、写在后面
部署已经完成，接下来就是一关一关的学习，和尝试了。<br/>
有空再记录第一关的笔记<br/>

**码字不易，对你有帮助的话，欢迎鼓励呀~**