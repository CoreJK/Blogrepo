+++
date = "2019-04-13T22:41:29+08:00"
categories = ["Git"]
tags = [""]


title = "Git使用采坑记"
description = "没有思考，照搬命令行的结果"
images = []
+++

# 前言
一直没空写博客,都忘了Git命令的使用<br>
在本地换了博客头像，准备上传<br>
输入了如下命令

``` git
git remote add origin git@github.com: CoreJK/corejk.github.io
```

# 报错信息
``` git
fatal: remote origin already exists.
```
# 原因
重复操作，这个指令是用于首次，将本地仓库，推送到Github上新建立的仓库中用的指令<br>
往后在本地更新博客内容后，上传到Github服务器时<br>
只用使用如下命令
``` git
git push -u origin master
```
这样就解决了问题
