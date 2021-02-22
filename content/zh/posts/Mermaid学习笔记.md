---
title: "Mermaid学习笔记"
date: 2021-02-22T22:12:35+08:00
description: "借助 Mermaid 语言可以在Markdown文档中进行流程图、时序图、类图、ER图的绘制，辅助快速撰写开发文档、理清程序开发流程，同时有助于同行交流，提升开发效率"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
libraries:
- mermaid
tags:
-
series:
-
categories:
-
image:
---

![开发设计用图](https://ftp.bmp.ovh/imgs/2021/02/e0c93b19bffe2efd.png)

## 前言

实习一个多月，终于要开始根据需求开发功能了

组长安排下来任务，让我们先写功能设计文档，再开始 Coding

这，写功能，不是拿起键盘就 Coding？？？！！！

不过写到一半，发现思路混乱（自己太菜），只好放下 Pycharm 中没有写完的代码，开始补开发文档

于是一边用 Typora 码字，一边用 Draw.io 画流程图、顺序图... ...(痛苦面具

好在 Typora 支持 美人鱼（Mermaid），官方仓库连接在下方😀

- [Mermaid Github](https://github.com/mermaid-js/mermaid)



## 好的文档为什么需要画图🙃

一图胜千言，适量的文字加上合适图表，可以实现 `1+1 > 2`的效果

引用官网的话

> "Diagramming and Documentation costs precious developer time and gets outdated quickly. 
>
> But not having diagrams or docs ruins productivity and hurts organizational learning."

> 绘制图表和文档花费了宝贵的开发人员时间，并且很快就过时了。 
>
> 但是没有图表或文档会极度拖慢生产效率并团队之间的交流学习造成不利影响。

1. 很快过时的原因是，绘图软件通常导出的文件是图片；

   软件开发过程中，在旧功能上做了修改

   更新对应文档的时候，想要编辑当中的图片就不是这么容易了。

2. 没有图表和文档，又是万万不能的

   作为一名新人，深知看图还是比纯文字文档省力

   好的开发文档中的图标，能够帮助理解抽象的功能实现，以及业务流程

   

## Meimaid 可以画哪些图？

如图所示

![支持的部分绘图](https://ftp.bmp.ovh/imgs/2021/02/7597cdb74032b146.png)

当然不止这些，可以去官网文档详细了解，相信一定不会枯燥的！😏

### 举例

- 流程图

```mermaid
graph TD
A[Hard] -->|Text| B(Round)
B --> C{Decision}
C -->|One| D[Result 1]
C -->|Two| E[Result 2]
```

- 顺序图

```mermaid
sequenceDiagram
Alice->>John: Hello John, how are you?
loop Healthcheck
    John->>John: Fight against hypochondria
end
Note right of John: Rational thoughts!
John-->>Alice: Great!
John->>Bob: How about you?
Bob-->>John: Jolly good!
```

- 状态图

```mermaid
stateDiagram-v2
[*] --> Still
Still --> [*]
Still --> Moving
Moving --> Still
Moving --> Crash
Crash --> [*]
```

- 类图

```mermaid
classDiagram
Class01 <|-- AveryLongClass : Cool
<<interface>> Class01
Class09 --> C2 : Where am i?
Class09 --* C3
Class09 --|> Class07
Class07 : equals()
Class07 : Object[] elementData
Class01 : size()
Class01 : int chimp
Class01 : int gorilla
class Class10 {
  <<service>>
  int id
  size()
}
```

- 实体关系图(实验阶段)

![实体图](https://ftp.bmp.ovh/imgs/2021/02/a272f3eec259bfb5.png)

- 甘特图

```mermaid
gantt
section Section
Completed :done,    des1, 2014-01-06,2014-01-08
Active        :active,  des2, 2014-01-07, 3d
Parallel 1   :         des3, after des1, 1d
Parallel 2   :         des4, after des1, 1d
Parallel 3   :         des5, after des3, 1d
Parallel 4   :         des6, after des4, 1d
```

上面这些图表，不用拖拽图片，睁大眼睛去微调

只用在支持 Mermaid 的编辑器环境中，编写几行 Mermaid 代码就可以自动绘制

如果熟悉 Markdown 语法，上手会很快

具体可以去[Mermaid官网文档](https://mermaid-js.github.io/mermaid/#/)学习~



## 写在最后

掌握了 Mermaid 的语法，结合支持 Markdown & Mermaid 的文本编辑器

作为开发人员编写开发文档

不用操心图片有没有对齐、线的粗细合不合适等（这些都是以后完善文档再去微调的事情😫

最少可以缩短三分之二用在绘图软件画图的时间

---


不过有一说一，毕竟 Mermaid 还在发展和完善中，无法满足更加细化的绘图需求

Mermaid 适合快速实现流程图、顺序图等场景

所以追求完美图表，而且时间充裕的人来说，还是应该考虑专业的绘图工具

