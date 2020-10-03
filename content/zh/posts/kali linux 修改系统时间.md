+++
date = "2020-02-28T17:07:08+08:00"
categories = ["Linux"]
tags = ["kali linux","系统时间"]


title = "kali_Linux 修改系统时区"
description = "修改系统时区的两个方法"
images = []

+++

## 查看当前系统时间

```shell
data -R
```

## 修改时间指令如下

**改指令适用于基于 Dbian 发行版的 Liunx 系统**

```shell
dpkg-reconfigure tzdata
```
## 选择 Asia 之后，再选择重庆（上海）即可

通过 “↑ ↓” 方向键选择地区，选好后，tab 键光标移动到 < 确定 > 并回车

![打开时间设置](https://ae01.alicdn.com/kf/Uf361ab7b65a44cb7b0c04ddb9dd51f9e8.jpg)

![选择地区](https://ae01.alicdn.com/kf/Ua0027c147fd4490d9e981cc09ce1e854V.png)

## 现在系统时间就同步为为东八时间了

![修改成功](https://ae01.alicdn.com/kf/U9ad4a26de7b74f46be9f9f514ff4cd15U.png)


**上面是基于图形化修改，还有基于命令行修改的指令**

**按照提示，选择对应数字修改**

```shell
tzselect
```

![tzselect1](https://ae01.alicdn.com/kf/Uc137135e46814edfb68ee18c41186364h.png)

![tzselect2](https://ae01.alicdn.com/kf/U82d6bac26e864d6ca337c6baa4d2318aN.png)

![tzselect3](https://ae01.alicdn.com/kf/Ufb44130bf1234ecba50d655bcf110797D.png)