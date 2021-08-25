---
title: "Hugo将主题添加为子模块"
date: 2021-01-21T13:46:27+08:00
description: "好久没有管理博客了，发现之前添加主题的姿势不正确"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "Hugo"
categories:
- "Git"
series:
- "工具使用"
image:
---

首先，我们需要把博客从仓库里拉取到本地

删除原有的子模块配置，以及清空子模块所有内容

然后再将新的Hugo主题仓库，添加到自己的博客仓库 "themes/" 文件夹中，作为子模块

## 一、远程拉取子模块到主项目

把博客从自己仓库拉取下来，发现主题没有一起被拉取

正确做法如下，到博客的根目录下执行下面的命令

```bash
git submodule update --init --recursive
```

## 二、彻底删除项目中的子模块 ( deinit )

**ps:以下命令均在博客的根目录下执行**

- 从 .git/config 配置文件中删除子模块配置的命令

```bash
`git submodule deinit -f path/to/submodule`
# 示例操作：我的旧主题的子模块所在位置
`git submodule deinit -f themes/zzo`
```

- 从 .git/modules 目录下，移除子模块的相关记录 

```bash
`rm -rf .git/modules/path/to/submodule`
# 示例操作
`rm -rf .git/modules/themes/zzo`
```

- 清空本地子模块文件夹下的所有文件

``` bash
`git rm -f path/to/submodule`
# 示例
`git rm -f themes/zzo`
```

## 三、删除后，添加子模块

```bash
git submodule add https://github.com/zzossig/hugo-theme-zzo.git themes/zzo
```

## 四、以后更新子模块

```bash
git submodule update --remote --merge
```



## 参考文章

> [Git删除子模块和远程分支 | 拾荒志 ](https://murphypei.github.io/blog/2018/09/git-delete-submodule)
>
> [笔记 | Y4er的博客|note/#git](https://y4er.com/note/#git)

