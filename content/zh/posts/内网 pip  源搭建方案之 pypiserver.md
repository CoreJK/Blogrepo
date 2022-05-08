---
title: "内网 Pip  源搭建方案之 Pypiserver"
date: 2022-05-08T16:46:23+08:00
description:
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "python"
categories:
- "编程语言"
series:
- "技术研究"
image:
---

## 前言

遇到开发环境是内网，项目第一次搭建的时候
需要安装许多的python三方模块
除了手动一个个安装，有时候还要处理相互依赖的模块，费时费力
能不能像在 **外网** 环境一样，在 **内网** 使用 **pip** 优雅的下载、安装模块~
最好还能让内网同事一起使用的方案呢？

## 自建 pip 源方案

没有枪炮，那就自己造嘛~
目前我找的方案有下列四种，按简单到复杂排列
根据实际情况，我只用到了**第一种**，是前三个中最简单的
**第四种**需要大容量的存贮，暂不考虑

1. [pypiserver](https://pypi.org/project/pypiserver/#quickstart-installation-and-usage)
2. [pip2pi](https://pypi.org/project/pip2pi/)
3. [devpi](https://devpi.net/docs)
4. [bandersnatch](https://pypi.org/project/bandersnatch/)

## 使用 pypi-server 创建 pip 源

使用 pypi-server 创建私有 pip 源。
该方法简便快捷，但是需要手动下载所需要的安装包，并上传到 pypiserver 所在的服务器
适合项目和初期使用
Python 的版本要求是：3.6+

### 1. 服务器安装 pypiserver

去外网 [pypi](https://pypi.org/project/pypiserver/#files) 下载 `pypiserver-1.5.0-py2.py3-none-any.whl`
传入内网机器，然后安装

```bash
pip install pypiserver-1.5.0-py2.py3-none-any.whl
or
pip3 install pypiserver-1.5.0-py2.py3-none-any.whl
```

### 2. 服务器新建模块包存放位置

以后传入内网的模块，都统一上传到部署了 `pypiserver` 的服务器上
这样团队中有人上传一次后，其他人需要该模块，就不用再花时间去外网找了~

```
# 创建安装包存储文件夹
$ mkdir /root/home/packages
```

### 3. 服务器配置运行 `pypiserver` 

- 方案一：手动开启服务，服务器重启失效
```bash
# 建议指定端口和文件夹启动服务，避免冲突
pypi-server -p 9090  /root/home/packages

# 不添加参数,默认使用 8080 端口和 packages 文件夹
pypi-server
```

- 方案二：配置 systemd ，便于管理 `pypiserver` [^1]，实现自启
  新建文件 `/etc/systemd/system/pypi_server.service`

```bash
[Unit]
Description=A minimal PyPI server for use with pip/easy_install.
After=network.target

[Service]
Type=simple
# systemd requires absolute path here too.
PIDFile=/var/run/pypiserver.pid
User=www-data
Group=www-data
# 启动命令&日志文件存放位置
ExecStart=/usr/local/bin/pypi-server -p 9090 -a update,download --log-file /var/log/pypiserver.log /root/home/packges
ExecStop=/bin/kill -TERM $MAINPID
ExecReload=/bin/kill -HUP $MAINPID
Restart=always
# 模块存放地址
WorkingDirectory=/root/home/packages

TimeoutStartSec=3
RestartSec=5

[Install]
WantedBy=multi-user.target
```

启用配置 & 管理

```bash
systemctl enable pypi_server

# 后期维护
$ systemctl status pypi_server    # 查看进程状态
$ systemctl stop pypi_server    # 终止 pypi_server 进程
$ systemctl start pypi_server    # 启动 pypi_server 进程
$ systemctl restart pypi_server    # 重新启动 pypi_server 进程
```

浏览器地址栏，输入 `http://10.10.10.10:9090/simple`
看到欢迎界面
![Welcome to pypiserver](https://s2.loli.net/2022/05/08/Vdplf7NnZ5HjziS.png)

### 4. 客户端修改 pip 默认源

假设 `pypiserver` 部署在了 10.10.10.10 的内网服务器上
我们需要修改自己电脑的中 `pip.conf` 文件，通过 `pip cofnig set` 命令修改

- 配置的语法是：`pip config set [name] [value]`
```cmd
# 直接修改 pip 源地址为 pypiserver 服务器所在地址
pip config set global.index-url http://10.10.10.10:9090/simple
or 
# 如果不想替换 pip 默认源
pip config set global.extra-index-url http://10.10.10.10:9090/simple

# 添加信任 pypiserver 服务器
pip config set global.trusted 10.10.10.10

# 查看修改结果：pip config list
global.index-url='http://10.10.10.10:9090/simple'
global.trusted-host='10.10.10.10'
```

### 5. 内网客户端使用 pip

无法连接外网的内网计算机，作为客户端使用服务器的pip源下载包
假设服务器的 IP 地址为 `10.10.10.10`

- 已经参考步骤 4 配置了 `pip.conf` 可像在外网一样使用 pip 的
```cmd
pip install numpy
```

或者使用下面的命令，可以不用配置 `pip.conf` 文件
建议参考 4 配置 `pip.conf`，一劳永逸

```cmd
pip isntall numpy --index-url http://10.10.10.10:9090/simple --trusted-host 10.10.10.10
```



[^1]: https://github.com/pypiserver
