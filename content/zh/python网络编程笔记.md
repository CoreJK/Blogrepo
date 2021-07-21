---
title: "如何判断不同主机是否在同一个网段？"
date: 2021-07-21T22:18:57+08:00
description:
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "网络编程"
- "python"
series:
- "python"
categories:
- "工作笔记"
image:
---

## 场景

现在我们知道 A 主机和 B 主机的 IP 地址以及子网掩码

如何写一段python代码判断两台主机的网络是在同一个网段下呢？

## 解决方案

在已知主机的 IP 和 子网掩码的前提下，我们只需要做一个简单`按位与`运算即可。

首先我们心算一下二进制

### ①  将 IP 地址、子网掩码转换为二进制

```bash
A 主机的网络信息
IP: 192.168.1.1 ---------> 11000000.10101000.00000001.00000001
Netmask: 255.255.255.0 --> 11111111.11111111.11111111.00000000

B 主机的网络信息
IP: 192.168.1.66 --------> 11000000.10101000.00000001.00000001
Netmask: 255.255.255.0 --> 11111111.11111111.11111111.00000000
```

### ②  两个主机分别将各自的 IP 与 自己的子网掩码进行按位与运算

请在心中将每一个`0`和`1`对齐，然后默念口诀"有零为零，全一为一"... ...

根据我举的例子，A、B 主机的 IP 与子网掩码计算之后，得出的二进制值肯定是一样的（都是同一个网段下的设备）

如下所示：

> 11000000.10101000.00000001.00000000

按分割转换为十进制后就是

> 192.168.1.0

### ③ 最后一步，比较最后得到的值

如果得到的 IP 一致，则说明两台设备在同一个网段内，否则不在统一网段内

## python 代码实现

今天的主角，是`netaddr`模块

```python
from netaddr import *

def get_network(ipaddr, netmask):
	network_info = f"{ipaddr}/{netmask}" # "192.168.1.1/255.255.255.0"
	host_network = IPNetwork(network_info)
	return str(host_network.network)
```



1. 导入`netaddr`模块中的所有类（方便演示例子，实际工作中不建议，看场景而定）
2. 第4行代码，使用`IPNetwork`类接收一个格式为"`<ip>/<netmask>`"字符串后，实例化为`host_network`
3. 最后一行代码，`get_network`函数返回`host_network`实例的`netwrok`属性值（将`IP`与`Netmask`按位与后的十进制结果），并转换为字符串

## 结语

`netaddr`还有很多方便的功能，可以处理`IPV4、6`等网络编程中遇到的‘棘手’问题

这只是网络编程一个小的开胃菜，可以预感以后会越来越精彩dog。

> [netaddr官方文档](https://netaddr.readthedocs.io/en/latest/tutorial_01.html)









