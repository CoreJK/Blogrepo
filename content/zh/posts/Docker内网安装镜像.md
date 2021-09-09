---
title: "Docker内网安装镜像"
date: 2021-08-26T23:00:15+08:00
description: "在无法联网的内网设备中，加载需要的镜像"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
categories:
- "Docker"
series:
- "技术研究"
image:
---

## 前提

1. 一台能够连网的设备，同时安装了Docker
2. 能够联网设备的操作系统架构要与内网的设备一致（例如内网是x86_64的，那么能够联网的设备最好也是）

第二点要求不是强制的，只是为了减少前期学习成本，快速上手。

**因为默认情况下，部署了docker的设备是什么架构，获取的镜像就是适合当前架构版本的镜像**

`docker pull` 命令有办法拉取不同架构、版本的镜像，以后再记录。

## 获取镜像

在联网设备的中，获取需要的镜像

```bash
docker pull <镜像名>:<Tag>
示例：
docker pull nginx:latest # 可以不带`:latest`，docker会默认获取latest(最新)镜像版本
```



## 打包镜像

### ① 获取当前已有镜像信息命令

```bash
docker images
-------------------------------------------------------------------------------
REPOSITORY     TAG        IMAGE ID            CREATED             SIZE
nginx          latest     dd34e67e3371        9 days ago          133MB
```

### ② 将目标镜像打包的命令

```bash
docker save <REPOSITORY>:<TAG> -o <指定宿主机器文件路径/任意名称.tar>
示例：
docker save ndingx:latest -o /root/nginx.tar
```

**注意`<REPOSITORY>:<TAG>`按照上面指令获取的镜像信息填写。**
否则，在内网中加载镜像包后，执行`docker images`后，会得到如下的输出，不便于后期升级维护。

```bash
REPOSITORY      TAG         IMAGE ID       CREATED        SIZE
<none>         <none>     dd34e67e3371    1 days ago      133MB
```



## 加载镜像

### 将镜像包发送给内网机器，加载镜像

```bash
docker load -i <打包镜像名.tar>
docker images # 查看是否安装成功
```



## 总结

这样就完成了内网Docker机器，获取镜像的操作。

后续再执行`docker run`等操作，启动容器。
