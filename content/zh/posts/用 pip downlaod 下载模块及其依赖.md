---
title: "用 Pip Downlaod 下载模块及其依赖"
date: 2022-05-29T22:46:55+08:00
description: "用 pip Downlaod 下载模块及其依赖，一步到位，下载需要的模块及其依赖，然后传入内网使用"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "python"
categories:
- "编程学习"
series:
- "技术研究"
image:
---

## 为什么要用 pip download ?

如果你是一名内网 python 开发者，当你发现项目的某个模块缺少了，你会怎么做？
如果内网没有 pip 镜像站点，那么只能：

1. 打开 pypi.org ，然后下载模块
2. 一个个找到模块的其他依赖模块
3. 传入内网，再安装... ...😥

很不幸，如果一个模块有十几个依赖，那就是一次痛苦的经历了😕

其实，可以用 python 自带的包管理器 `pip` 的 `download` 命令~
帮我们下一步到位，下载需要的模块及其依赖，节省不少时间

## 一、指定模块下载路径

切换到可以上网的机器
这里我使用的环境是

- **操作系统**：windows 10 64 bit  -- 缩写：win64
- **python 版本**：3.9  -- 缩写：39
- **解释器 版本** : cpython  -- 缩写：cp

例如，我们需要下载 csvkit 模块及其依赖，到我桌面的 csvkit 文件夹中

```bash
mkdir C:\Users\username\Desktop\csvkit
# 一定要先创建好目录，用于存放模块和它的依赖模块
pip download <模块/包名> -d <模块/包的存放路径>
pip download csvkit -d C:\Users\username\Desktop\csvkit 
```

> 默认情况下`pip download` 会根据**当前操作系统、python 版本、python 解释器版本**
将**目标模块、以及依赖的模块**，都下载到 `-d` 参数指定的路径下


**如果，你仅仅需要下载 csvkit 模块，而不需要该模块的其他依赖**

```bash
pip download csvkit 
  --no-deps 
  -d C:\Users\username\Desktop\csvkit
```

如果**内外网的机器**的【操作系统、python 版本、python 解释器版本】都一致
只需要使用上述的命令足以

如果【操作系统、python 版本、python 解释器版本】大多和外网机器不同，要怎么办呢？
有时我们还可能需要**特定文件类型的安装包**【`*.whl`（编译过的二进制）、`*.tar.gz 或者 *.zip`（源码安装包）】


## 二、根据操作系统、python 版本、python 解释器版本下载模块

因此，需要了解 `pip download` 的其他参数[^2]，来应对**内外网环境不一致**的情形

> 关于 python 的二进制包和源码包的区别[^1]：l
> 1. 二进制包（Binary Packages），通常是 `*.whl`文件格式，需要通过 `pip install *.whl` 安装。可以把 python 的二进制包，比作可执行包或 Windows 的 msi 安装程序；
> 2. 源码包（Source Packages）通常是`*.tar.gz`、bz2 、`*.zip`格式的文件，通常需要解压后，通过 `python setup.py` 进行编译安装（也可以直接 `pip install *.tar.gz`）

```bash
pip download csvkit 
  --only-binary=:all:  # 模块文件类型过滤器
  --platform win32  # 操作系统的版本
  --python-version 36  # python 3.6
  --implementation cp  # 还可以是 pypy 等不同解释器，默认是 cpython
  -d C:\Users\coreJK\Desktop\csvkit
```

>  当使用 --platform（操作系统）、--python-version（python 版本）、--implementation或--abi（解释器版本），作为限制条件时，必须设置--no-deps（不下载所依赖的模块），或者设置--only-binary=:all:（），并且不能设置--no-binary（或者必须设置为--no-binary=:none: ）


## 提示

如果项目不需要经常的更新模块，可以在内网部署好 `pypiserver`（便于在内网大家一起使用 `pip` 安装模块）
借助 `pip download` 命令，从外网下载好模块和依赖
再一并传入内网的 `pypiserver`

如果项目频繁更新模块，可以考虑配置自建内网的 pip 镜像站点
根据实际情况，我只用到了**第一种**，是前三个中最简单的
**第四种**需要大容量的存贮，暂不考虑

- [pypiserver](https://pypi.org/project/pypiserver/#quickstart-installation-and-usage)
- [pip2pi](https://pypi.org/project/pip2pi/)
- [devpi](https://devpi.net/docs)
- [bandersnatch](https://pypi.org/project/bandersnatch/)


[^1]: [pip_download 命令手册](https://pip.pypa.io/en/stable/cli/pip_download/)
[^2]: [Binary 和 Source 包的异同](https://www.linuxquestions.org/questions/linux-newbie-8/binary-and-source-packages-difference-860666/)