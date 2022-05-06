---
title: "Python 实现 Windows 远控"
date: 2022-05-06T20:28:35+08:00
description: "借助 pywinrm 模块，实现脚本远控 windows 的目的"
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
- “技术研究”
image:
---

## pywinrm 模块
windows 除了通过图形远程访问
其实也可通过命令行访问
python 借助 pywinrm 模块
可以实现远程访问 Windows 的 cmd、powerShell
执行 dos 命令 ，达到远程管理的目的


## 控制端安装 pywinrm

环境要求[^1]
- Linux, Mac or Windows
- Cpython 2.6 - 2.7, 3.3-3.5 or PyPy2

一、**外网**

```bash
pip install pywinrm
```

二、**内网**
根据系统，下载合适的离线安装包

- [pywinrm 离线安装包](https://pypi.org/project/pywinrm/#files)
```bash
pip install pywinrm-0.4.3-py2.py3-none-any.whl
```

## 被控端 windows 开启 winrm 服务

> 被控制系统：windows 7 

1. **检查服务监听情况**
```cmd
winrm enumerate winrm/config/listener
or
winrm e winrm/config/listener

# 已经开启回输出如下信息，未开启则无信息输出
Listener
	Address = *
	Transport = HTTP
	Port = 5985
	Hostname
	Enabled = true
	URLPrefix = wsman
	CertificateThumbprint
	ListeningOn = 10.10.10.10, 127.0.0.1, ::1, ...
	...skip...
```

2. **启动 winrm 服务**

先用管理员权限，运行 cmd
再执行下面的命令
```cmd
winrm quickconfig -q  # 静默启动
```

检查是否启动
```cmd
winrm e winrm/config/listener
or
netstat -ano | findstr 5985
```

## 查看 config 信息
几个基本的配信息查询命令
根据自己需要，查询需要配置的字段

1. AllowUnencrypted
2. Basic
3. TrustedHosts

- 查看 config：`winrm get winrm/config`
包含了 Client 、Service、Winrs 的信息
```bash
Config
	...skip...
	Client
		...skip...
	Service
		...skip...
	Winrs
		...skip...
```

- 只查看 Client：`winrm get winrm/config/client`
```bash
Client
	NetworkDelayms = 5000
	URLPrefix = wsman
	AllowUnencrypted = true
	AUTH
		Basic = true
		Digest = true
		...skip
	DefaultPorts
		HTTP = 5985
		HTTPS = 5986
	TrustedHosts = *
```


- 只查看 Service：`winrm get winrm/config/service`
```bash
Service
	...skip...
	AllowUnencrypted = true
	AUTH
		Basic = true
		Digest = true
		...skip
	DefaultPorts
		HTTP = 5985
		HTTPS = 5986
	TrustedHosts = *
```

**忘记有那些参数要配置了，或是需要确认字段配置生效，运行上述命令检查即可。**

## 配置 winrm service

```bash
winrm set winrm/config/service/ @{AllowUnencrypted="true"}
winrm set winrm/config/service/auth @{Basic="true"}
```

## 配置 winrm client

```bash
winrm set winrm/config/client @{AllowUnencrypted="true"}
winrm set winrm/config/client @{TrustedHosts="*"}
winrm set winrm/config/client/auth @{Basic="true"}
```

## 基本用法
配置好上述字段后，我们就能通过 `pywinrm` ，像 `paramiko` 模块通过ssh一样
远程执行 cmd、powerSehll 命令了

```python
import winrm
s = winrm.Session('10.244.14.24:5985', auth('username', 'passwd'), transport='ntlm')
r = s.run_cmd('whoami')
r.status_code  # 0 为正常
r.std_out  # bytes 得到的正常执行结果
r.std_err  # bytes 无异常消息为空字节串
```

[^1]: https://pypi.org/project/pywinrm/
