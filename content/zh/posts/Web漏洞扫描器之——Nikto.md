+++
date = "2019-07-22T15:39:27+08:00"
menu = ""
categories = ["渗透学习笔记"]
tags = ["渗透工具"]

banner = "banners/cyber.jpg"
title = "Web漏洞扫描器之——Nikto"
description = "主动扫描工具"
images = []
+++

针对 Web Application 部分的扫描工具
**目标均为虚拟机中靶机**
以下只是基本用法，太多无法一一记录。
有需要再用查看文档即可


# 基本命令
```
nikto -update
nikto -list-plugin
```
- 检查更新
- 显示可用插件

##  一、常用
**扫描目标的IP或URL，端口默认为 80**
```
nikto -host <IP&URL>
```
![基本用法](https://ae01.alicdn.com/kf/U1d9f29cafdcb462795cc52f1cb05ad65W.png)

## 二、指定目标的端口
```
nikto -host <IP&URL> -port 80,22,25,445
```
![指定端口](https://ae01.alicdn.com/kf/U28f2614e75b44af5bbaae0e75d8ff3c5K.png)

## 三、目标网址采用https连接时，带上-ssl
```
nikto -host <IP&URL> -port -ssl
```
![-ssl](https://ae01.alicdn.com/kf/U8d19d48b93a44fb1a30927f63e1e49283.png)

## 四、防止被Ban，使用匿名代理服务器
```
nikto -host <IP&URL> -port -ssl -useproxy http://127.0.0.1:9999
```
## 五、防止被IDS检测异常请求
**-evasion 选项有多种混淆方式，输入数字选择**
```
nikto -host <IP&URL> -port -ssl -useproxy <服务器IP:port> -evasion 167
```
![evasion](https://ae01.alicdn.com/kf/Ue41e29223d5846dfbd34f73c836708a4T.jpg)
选择了167，表示“随机URL(非UTF-8)编码”，“TAB 替换空格”，“改变URL大小写”

## 六、配合nmap使用，扫描同一网段，多个主机
```
namp -p80 10.10.10.0/24 -oG - | nikto -host -
```
将 nmap 的扫描到开放了 80 端口的目标，传给 nikto 作为 -host 的输入。

## 七、扫描结果以某种格式输出
```
nikto -host 10.10.10.129 -F htm -o htm
```
结果，以 html 格式输出，方便查看
![html](https://ae01.alicdn.com/kf/Ud4139e7f558f414abfb270696c670aeen.jpg)

# 总结
1. nikto 不支持填写表单验证，这意味着要测试目标站点如果有登陆验证，能获得信息比较有限。
2. 工具返回的信息，也不一定百分百准确，要人工验证
3. 能和 namp 结合使用
4. 支持扫描结果以多种格式保存
5. 有许多插件可以调用