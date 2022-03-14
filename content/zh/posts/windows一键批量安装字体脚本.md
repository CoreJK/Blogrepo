---
title: "Windows一键批量安装字体脚本"
date: 2022-03-14T19:32:38+08:00
description:
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "Joplin"
- "bat"
- "字体"
categories:
- "Windows"
series:
- "工具使用"
image:
---

 # 前言

不知道大家有没有过，因为工作软件，或者美化系统的需要
『**一个个**』点击安装字体文件的恐惧支配过
**我在折腾 Jopin 主题的时候，由于不同的主题，用到的字体不同，往往需要花很多时间精力去**
『**一个个**』选择，然后点击字体文件安装

不多还好，如果依赖的字体越多，安装字体的过程，简直就是一个噩梦... ...
好在有更好的解决方案，解放我们的双手，只要一个程序，和几段小小 `bat` 代码
开始把！

## 一、下载 Fontreg.exe 程序

> Fontreg.exe 是一个可执行文件，没有图形界面。它可以帮助我们复制、并注册安装字体文件
> 结合脚本代码(bat、python... )，可以实现批量安装字体文件

1. 点击☞[Fontreg-2.1.3地址](https://code.kliu.org/misc/fontreg/fontreg-2.1.3-redist.7z)下载压缩包
2. 你看上的字体文件佳丽们~的压缩包，或者单个字体文件

## 二、将 Fontreg.exe 添加至环境变量

> 环境变量，听上去高大上，可以简单理解为某个程序或者文件『快捷方式』
> 便于我们在任何路径下，去调用添加了快捷方式的『程序或文件』
> 在 Linux 操作系统下，还有个好东西叫做『软链接』，和快捷方式很像，但是不能和『环境变量』搞混了

为了脚本运行方便，我们将解压得到的 `Fontreg.exe` 所在路径，添加到系统的环境变量当中
首先，将 `Fontreg.exe` 所在路径复制下来。

> 我演示的操作系统是64位，如果你的系统是32位，请复制 `x:xx\xx\bin.x86-32\` 的路径信息
> 不然后面运行脚本会出错

![](https://s2.loli.net/2022/03/14/vjWul4HIZLpYArs.gif)

然后，添加到环境变量`path`中

> 记得要逐一点击 `确定` ，才能使修改生效！

![](https://s2.loli.net/2022/03/14/B26m5NbF1DChVHJ.gif)

>比较新的 win10系统环境变量入口如下图所示，打开后按照上面的步骤添加即可

![](https://s2.loli.net/2022/03/14/VzmsS7GuJvW3l4d.gif)


## 三、编写脚本

在解压得到的字体文件夹`c:\xx\Font-ChironSansHKPro-ExtraLight\`根目录下，右键新建一个文本文档，按图片重命名为`install_fonts.bat`

![新建一键安装字体脚本](https://s2.loli.net/2022/03/14/YEi8haekJpqc2mM.png)

然后右键通过编辑打开它，将下面的代码复制粘贴进去，并保存。

> 第二行代码，是给 bat 脚本添加了管理员权限，因为脚本需要把字体文件，复制到系统的`C:\Windows\Fonts` 目录下，需要较高的权限

```bat
@echo off
%1 mshta vbscript:CreateObject("Shell.Application").ShellExecute("cmd.exe","/c %~s0 ::","","runas",1)(window.close)&&exit
cd /d %~dp0

set fontExtensions=(ttf otf)
for %%x in %fontExtensions% do (
	echo A|xcopy %cd%\*.%%x %windir%\fonts\
) 
for /f "delims=" %%i in ('dir /ad/b/s "%cd%"') do (
	for %%x in %fontExtensions% do (
		echo A|xcopy %%i\*.%%x %windir%\fonts\
	) 
)

echo "registering font..."
FontReg.exe
echo "Registering complete!"
pause
```

> 有兴趣研究了脚本源码的小伙伴，可能发现我改动了倒数第三行的内容
> 是的，脚本作者没有将 `FontReg.exe` 添加到系统变量，所以需要 `%cd%`，并且需要带着 `FontReg.exe` 和脚本一起跑，我在这个基础上做了一些修改。

## 四、通过脚本一键安装字体

1. 把`install_fonts.bat`放置到字体文件所在根目录
2. 双击 `install_fonts.bat` 

> 脚本就会听话的帮我们，把『当前目录』以及『子目录』下的所有后缀为 `*.ttf` 、`*.otf`的字体文件
> 复制到 `C:\Windows\Fonts`目录下
> 并通过 `FontReg.exe` 字体注册工具，安装到系统中供我们使用了。

例如，我们先用这个脚本安装  `Font-ChironSansHKPro-ExtraLight` 字体，安装之前打开`C:\Windows\Fonts`，可以看到，是没有`ChironSansHKPro-ExtraLight`字体的

![](https://s2.loli.net/2022/03/14/vWZFO5mMgIeuHQk.png)

将脚本放置到字体所在目录，双击运行：
![安装字体](https://s2.loli.net/2022/03/14/UIwZDLujJPsYWov.gif)

运行完成后可以检查下字体有没有安装成功！
![成功安装 Chiron Sans HK Pro 特细](https://s2.loli.net/2022/03/14/XfKkMx5bdG4W9hJ.png)

脚本会递归查看所有当前目录下的所有子目录，然后逐一复制，然后安装字体。
![](https://s2.loli.net/2022/03/14/EdPh8GV6ZAa9Wuy.gif)



## 五、妖精的尾巴

虽然，通过这种方式可以很方便的**一键安装字体**
但是，有时并不需要，安装所有的字体文件
用不到的字体，不仅浪费空间（有些字体文件很大），而且影响字体安装的效率。

> 当然可以把需要的字体留下，然后用再用脚本安装字体，也是可以的

## 挖个坑

也许可以用  `python`  写一个程序（命令行？GUI？）
目的是，可以方便的选择需要安装的字体，然后一键安装
多是一件美逝啊~

## 参考文章

1. [Fontreg.exe的使用介绍](https://www.richud.com/wiki/Windows_Fontreg_install_fonts_locally_or_remotely_easily)
2. [`autoinstall.bat`脚本源码地址](https://github.com/MichaelIT/autoinstallfont/blob/master/autoinstall.bat)
