---
title: "Django 部署笔记"
date: 2022-02-14T10:26:39+08:00
description: "把 Django 应用部署至阿里云上线"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "python"
- "Django"
categories:
- "Web开发"
series:
- "技术研究"
image:
---

<img src="https://s2.loli.net/2022/02/14/YDIaf9QnL8qNKFT.png" style="zoom:60%"/>

## 背景

花了大概一个月左右的时间，跟着一个大佬的教程，完成了用 『 Ubuntu + Nginx + Gunicorn +DJango 2.2 + python3 + sqlite3』 本地构建博客(论坛)项目，并在阿里云上部署上线的流程。

教程内容不能说全部都掌握了，短短一个月，也不可能把所有技术栈掌握，毕竟教程涵盖了全栈开发的基本内容，还有好多内容在学... ...

未来可能还会用 Django 构建不同的应用，但基本部署流程相通，做个记录，便于以后部署项目上线，少走弯路

## 服务器环境

![部署结构](https://s2.loli.net/2022/02/14/z5oYQjLbHem4cSP.jpg)

- 服务器：Ubuntu 16.04 TLS
- 反向代理：nginx/1.10.3 
- 『web服务』：Gunicorn/20.1.0
- Django 2.2
- python3.7
- Git

## 部署前的准备

### 项目主要结构

```bash
django_project
|-- env
|-- my_blog
| |-- article
| |-- comment
| |-- media
| |-- my_blog
| |-- static
| |-- templates
| |-- userprofile
| |-- db.sqlite3
| |--.env
| `-- manage.py
|-- README.md
|-- .gitignore
`-- requirements.txt
```



### 一、安全检查

Django 框架生成的项目，在项目上线部署前一定要检查的以下内容(不限，以后了解更多后再补充)：

1. Git 版本记录中是否有敏感信息、占空间的内容；

> 如果从项目创建初始就采用了 Git 进行版本控制，一定要检查历史 commit 记录中，是否有泄露和安全相关的配置『如数据库用户、密码、邮箱等』；不需要提交的内容是否写在 `.gitignore` 文件中『数据库文件、静态资源、媒体资源等』。

**参考(待补充笔记)**

2. `settings.py`中的重要信息是否脱敏处理；

> 最好在项目创建初期，可以通过 `django-environ` 等方法从应用程序的外部变量中，读取敏感信息；为了不让 Git 记录，所以 `.gitignore` 文件一定要编写好相应的规则。

**参考(待补充笔记)**

### 二、生成 requirements.txt

激活虚拟环境，本地运行再次测试

没有问题后，进入项目根目录，生成项目依赖库信息

```bash
pip freee > requirements.txt
```

### 三、上传项目代码至Github

创建远程仓库后，将本地的代码 `push`到仓库

```bash
git remote add origin master git@github.com:corejk/django_blog.git
git push -u origin master
```

我的电脑已经安装并配置好了Git 推送相关的设置。

如果没有安装需要先安装好 Git 并配置好远程推送。

**参考(待补充相关笔记)**

## 开始部署

### 一、代码部署

项目代码已经上传到了远程仓库，便可以通过 Git 拉取、推送了

安装好Git，并配置好和`ssh-key`

```bash
apt-get install git
```

**ssh-key 配置笔记待补充**


```bash
mkdir -p /home/sites/blog/
cd /home/sites/blog/
git clone https://github.com/CoreJK/django_blog.git
# 或者
git clone git@github.com:CoreJK/django_blog.git
```

> 通过 https 拉取代码会遇到如下报错，可以换成 git@github.com:xx 拉取代码
>
> fatal: unable to access... 

### 二、部署 DJango 环境

升级一下系统自带库的版本

```bash
~$ sudo apt-get update
~$ sudo apt-get upgrade
```

项目代码基于 python3，所以我们通过 pip3 安装 虚拟环境模块

```bash
pip3 install virtualenv
# 如果缺少 python3 则运行以下内容
apt-get install python3
apt-get install python3-pip
```

进入项目根目录，创建激活虚拟环境

```
virtualenv --python=python3.5 env
source env/bin/activate
(env) ../django_blog$
```

接下来就是安装库、收集静态资源、数据迁移了：

```bash
(env) ../django_blog$ pip3 install -r requirements.txt
(env) ../django_blog$ pip3 install Gunicorn
(env) ../django_blog$ python3 manage.py collectstatic
(env) ../django_blog$ python3 manage.py migrate
```

代码部署基本完成，Gunicorn 我们也在虚拟环境下安装好了，接下来安装配置 Nginx。

### 三、安装 Nginx

安装 Nginx 并测试能否正常访问

```bash
apt-get install nginx
service nginx start
```

打开浏览器，输入你的**服务器公网 IP 地址**，出现 `Welcome to nginx!`后继续下一步配置

```
cd /etc/nginx/sites-avaliable
vim my_blog
```

在 my_blog 文件中写入

```bash
server {
  charset utf-8;
  listen 80;
  server_name 156.15.24.31;  # 改成你的 IP

  location /static {
    alias /home/sites/blog/django_blog/collected_static;
  }

  location /media {
    alias /home/sites/blog/django_blog/django_blog/media;
  }

  location / {
    proxy_set_header Host $host;
    proxy_pass http://unix:/tmp/156.15.24.31.socket;  # 改成你的 IP
  }
}
```

> 此配置会监听 80 端口（通常 http 请求的端口），监听的 IP 地址写你自己的**服务器公网 IP**。
>
> 配置中有3个规则：
>
> - 如果请求 static 路径则由 Nginx 转发到目录中寻找静态资源
> - 如果请求 media 路径则由 Nginx 转发到目录中寻找媒体资源
> - 其他请求则交给 Django 处理『准确说是Gunicorn』

这一步编写的配置是可用配置，需要将它连链接到启用配置目录下

同时还需要多做一步操作，将默认的`default`配置禁用

```bash
ln -s /etc/nginx/sites-avaiable/my_blog /etc/nginx/sites-enabled/my_blog
mv /etc/nginx/sites-enabled/default /etc/nginx/sites-enabled/default.bak
service nginx reload
```

#### Gunicorn 测试

回到 `manage.py` 所在目录，激活虚拟环境，启动 Gunicorn

```bash
(env) ../django_blog$ gunicorn --bind unix:/tmp/156.15.24.31.socket my_blog.wsgi:application
```

可以访问你的站点了，我访问了站点中的一篇文章

输入`http://156.15.24.31/article/article-list/2/`

<img src="https://s2.loli.net/2022/02/14/YDIaf9QnL8qNKFT.png"/>

### 四、配置进程托管

配置进程托管的方法有多种，我目前选择的是`systemctl`方案

方案参考了大佬写的*[《Web 服务的进程托管》](https://frostming.com/2020/05-24/process-management/)*的文章

创建进程托管文件 `my_blog.service`，放在 `/etc/systemd/system/`目录下

`vim /etc/systemd/system/my_blog.service`

```bash
[Unit]
Description=My blog service

[Service]
Type=simple
# manage.py 所在目录
WorkingDirectory=/sites/blog/django_blog/my_blog/
# gunicorn 服务安装在虚拟环境，注意路径
ExecStart=/sites/blog/django_blog/env/bin/gunicorn --bind unix:/tmp/156.15.24.31.socket my_blog.wsgi:application
KillMode=process
# 进程意外退出后 10s 重启
Restart=on-failure
RestartSec=10s

[Install]
WantedBy=multi-user.target
```

然后启用改配置

```bash
systemctl enable my_blog
```

这样你的进程就自动加入开机自启动了，同样，systemd 也可以查看、启动、停止进程：

```bash
$ systemctl status my_blog    # 查看进程状态
$ systemctl stop my_blog    # 终止my_blog进程
$ systemctl start my_blog    # 启动my_blog进程
$ systemctl restart my_blog    # 重新启动my_blog进程
```

### 六、上线

配置好了进程托管

只需要运行，就可以关闭黑漆漆的命令行，从任何设备访问自己的博客了

```bash
 service nginx start
 systemctl start my_blog
```

当然，这还是一个很初级的测试站点，还有很多工作要做...

## 后续工作

### 添加普通用户

### 替换数据库

### 站点优化

### 后期运维

每次修改代码后，更新到服务器上也很简单。在**虚拟环境**中并**进入项目目录**，依次（collectstatic 和 migrate 是可选的）执行以下命令：

```bash
git pull origin master
python3 manage.py collectstatic
python3 manage.py migrate
# 重启服务
systemctl restart my_blog
```

### 申请域名

申请了域名后，所有ip 都要换成域名，切换为ip后会有问题











