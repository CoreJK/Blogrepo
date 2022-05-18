---
title: "Python 如何使用 Datetime 模块获取当天任意时刻"
date: 2022-05-18T23:35:57+08:00
description: "通过字符串拼接或者 datetime.timedelta 模块"
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

## datetime 的时间魔法

用过 python 中 datetime 模块的话

都知道可以通过 `datetime.now()` 或者 `datetime.today()` 获取到当前系统的`年、月、日、时、分、秒`

有没有办法，能在程序运行的时候，获取到系统当天的任意时刻呢？也就是`时：分：秒`

**有两种方案可以尝试，需要预热的知识：**

- datetime.datetime.now() or datetiime.datetime.today()  -- 获取系统当前时间
- datetime.strftime() -- 转化时间对象为指定格式的字符串
- datetime.strptime() -- 指定格式的字符串转化时间对象
- datetime.timedelta()  -- 通过增量，修改时间对象

## 一、字符串拼接法

两行代码实现
```python
#!usr/bin/python3
# -*- utf-8 -*-

import datetime

today = datetime.datetime.now().strftime("%Y%m%d")  # 20220518
dt_5_30 = datetime.datetime.strptime(today + ' 05:30', "%Y%m%d %H:%M")  # 20220518 05:30
dt_23_30 = datetime.datetime.strptime(today + ' 23:30', "%Y%m%d %H:%M")  # 20220518 08:30
```

一起来看看代码的实现思路：

1. 先通过 `datetime.strftime()` 方法得到当天的`年、月、日`的格式化字符串；
2. 然后通过 `datetime.strptime()`方法，当天的`年、月、日`字符串，拼接上指定的时刻，如：“05:30”。

**datetime 的 `strptime`方法，会将我们指定的时间，转换为一个`datetime`对象**



## 二、“时间滑块法” -- timedelta

如果把时间想象成一个从左往右的慢慢滑动的滑块... ...

> 那么，我们就可以任意把时间 “拨动” 到任意时刻了不是么？

我们需要借助 `timedelta` 的 “时间魔法”，把时间拨动一下，得到我们想要的时间

```python
#!usr/bin/python3
# -*- utf-8 -*-

from datetime import datetime, timedelta

def get_today_hour_minute(H, M, S=0):
    """ 可以拨动的 “时间滑块” 函数
    :param H: int hour 小时
    :param M: int minute 小时
    :param S: option int 秒数
    :return: 返回当天的任意时刻的 datetime 对象
    """
    today = datetime.today()  # datetime.now() 也可以，不过精度要求不高时可以用 today
    zero_today = today - timedelta(hours=today.hour, minutes=today.minute, seconds=today.second, microseconds=today.microsecond)
    set_today = zero_today + timedelta(hours=H, minutes=M, seconds=S)
    return set_today

dt_5_30 = get_today_hour_minute(5, 30)
dt_23_30 = get_today_hour_minute(23, 30)
```

一起来看看代码的实现思路，是怎么拨动时间的呢？

1. 通过 `datetime.today()` 得到当前的系统时间；
2. 借助 `timedelta`(表示时间的增量，用绝对值的思维)，让时间对象 `today` ，减去 “自己”已经 走过的时间，使得`today`时间 “归零”；
3. 让后，再通过 `timedelta` ，给清零的时间对象 `today` 一个新的增量，通过传入的参数 `H、M、S`。也就是，我们自己拨动了表盘的`时针、分针、秒针`

在这里，我们也能操控时间了 XD

## 三、使用场景

用上面的任意一种方法
得到的用于界定时间范围的`datetime`对象
就可以用于当天时间数据的对比了
举个小例子：

```python
if datetime.datetime.now() < dt_5_30:
	print("当前时间小于 05:30")
elif (datetime.datetime.now() >= dt_5_30 and datetime.datetime.now() < dt_23_30):
	print("当前时间大于等于 05:30，小于 08:30")
```

你喜欢哪一种方案呢？
