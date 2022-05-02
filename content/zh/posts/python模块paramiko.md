---
title: "Python模块paramiko"
date: 2022-05-02T21:18:00+08:00
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

日常开发连接 Linux 服务器，都会用到 ssh 客户端的软件
 其实 python 有一个模块，可以简单模拟一个 ssh 客户端/服务端
 编写一些自动化的的任务时，会经常和它打交道
 简单记录一下 paramiko 的基本使用

 ## 安装 paramiko
 paramiko 不是标准库，需要自行安装
 要求 `py2.7，3.4` 版本以上[^1]

 ```shell
pip install paramiko
 ```

 ## 实例化 ssh 客户端
```python
import paramiko
ssh_client = paramiko.SSHClient()
```

 ## 与服务器建立连接

在得到一个 ssh_client 客户端连接对象后

第一次和服务器建立连接，需要选择是否自动将服务器的 host_key 公钥，记录在本地

> Host-key: 主机密钥，是用于在SSH协议中对计算机进行身份验证的加密密钥。
>
> 主机键是钥匙对，通常使用RSA，DSA或ECDSA算法。公共主机密钥存储在和/或分发给SSH客户端，私钥存储在SSH服务器上。[^2]

因为，在计算机的世界里，没有用久的朋友-- IP 短时间里不会刷新，但是系统可能随时被重置

```python
ssh_client.set_missing_host_key_policy(paramiko.AutoAddPolicy())  # 添加未知主机的 ssh 公钥
ssh_client.connect(hostname='10.10.10.10', port=22, username='root', password='root')
```

否则会报错提示:

```python
paramiko.ssh_exception.SSHException: Server '10.10.10.10' not found in known_hosts
```

可以加入 logging 模块，查看详细的通讯过程

```python
import logging
logging.basciConfig(level=logging.DEBUG, format="%(asctime)s - %(levelname)s - %(message)s")
```

 ## 发送命令 

使用三个变量，获取返回的信息对象

- stdin：发送指令
- stdout：执行结果
- stderr：报错信息

```python
stdin, stdout, stderr = ssh_client.exec_command("pwd")
```



 ## 解析命令执行结果

读取命令的执行结果

```python
res, err = stdout.read(), stderr.read()
return_msg = res if res else err
result = result.decode(encoding="utf-8")
print(result)
# 结果
>>> '/root'
```

## 完整代码

```python
import paramiko
import logging

logging.basciConfig(level=logging.DEBUG, format="%(asctime)s - %(levelname)s - %(message)s")

ssh_client = paramiko.SSHClient()
ssh_client.set_missing_host_key_policy(paramiko.AutoAddPolicy())  # 添加未知主机的 ssh 公钥
ssh_client.connect(hostname='10.10.10.10', port=22, username='root', password='root')

stdin, stdout, stderr = ssh_client.exec_command("pwd")
res, err = stdout.read(), stderr.read()
return_msg = res if res else err
result = result.decode(encoding="utf-8")
print(result)
```



[^1]: https://docs.paramiko.org
[^2]: https://www.ssh.com/academy/ssh/host-key
