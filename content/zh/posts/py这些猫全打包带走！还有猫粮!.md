+++
date = "2019-07-22T16:01:16+08:00"
categories = ["python"]
tags = ["笔记"]


title = "这些猫全打包带走！还有猫粮！——zipfile模块"
description = "《python让繁琐的工作自动化》笔记"
images = []
+++

# 概述
zipfile模块，是python内置的，用于处理 zip 压缩文件
平常鼠标能干的，zipfile 提供的方法也都能干!
不过大多数人还是愿意用鼠标来干... ...（小声）

**优点，借助 python 中的其他<文件处理>的模块
根绝任务需要，构建脚本，可以实现例如备份重要数据的操作
可以处理大量的文件，减少出错的概率，防止丢失可怜的薪资，顺便带走所有宠物，不然要 python 有何用？**

# 使用方法
## 实例
![压缩文件cats.zip的内容](https://ae01.alicdn.com/kf/U9e0e05c0f98b4651969344f6f4297c2fx.png)

### 一、创建新的压缩文件，并添加内容
我们要把实例中的内容，添加到压缩文件中去，压缩文件命名为“cats.zip”
这里，我在命令行中操作，到实例所在的目录运行
```
#!python3
D:py\Auto
>>> import zipfile
>>> cats_zip = zipfile.ZipFile("cats.zip", 'w')
>>> cats_zip.write("cats", "spam.txt", compress_type=zipfile.ZIP_DEFLATED)
```
ok, 打包完成
**我用的 'w' 文件模式（写入），意味着，当cat.zip不存在时，则会创建；
但若存在，会清空同名的压缩文件内容，再写入新内容！
如果要添加文件，到已存在的压缩文件中，则文件模式要用 'a' （添加）**

### 二、读取压缩文件，查看包含的文件
现在我的cats_zip 的压缩文件对象还没有关闭，可以继续操作
```
>>> cats_zip.namelist()
>>> ['spam.txt', 'cats/', 'cats/catnames.txt', 'cats/zophie.jpg']
```
**通过.namelist()方法，我们得到了一个压缩文件内，包含的目录/子目录，以及文件名字的列表**

### 三、读取压缩文件内，文件的相关信息
所要读取的文件信息，必须在返回的列表中，否则会报错
现在来读取 spam.txt 的为压缩的大小，和压缩后的大小
```
>>> spamInfo = cats_zip.getinfo("spam.txt")
>>> spamInfo.file_size
>>> 53
>>>spamInfo.compress_size 
>>> 43
```
**注意，这里 spam.txt 是一个新的对象，文件大小是读取对象的“属性”，后面没有一对小括号**

### 四、解压全部内容，或者个别需要的文件
```
>>> cats_zip.extractall()
>>> cats_zip.extract('spam.txt')
```
- 第一个是解压全部内容，默认解压到当前文件夹
- 第二个是解压 cats.zip 中的 spam.txt ，默认解压到当前文件夹
- 我们可以指定解压的目标路径（绝对路径）
- 如果指定的文件路径（绝对路径）不存在，会自动创建 
```
>>> cats_zip.extractall("C:\\Desktop")
>>> cats_zip.extract('spam.txt', "C\\Desktop")
```
# 总结
- zipfile.ZipFile() 调用时模块名后，要注意是大写！
- python 中，文件是对象、zip是对象... ...而且两者的操作类似
- 打开后记得 **文件对象.close()**， 或者用 with ... as ...，就省心多了
- 以后可以用这个“拖库”再跑路... ...XD?
