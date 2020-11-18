---
title: tinypng 在vue中的使用
tags:
  - vue
  - sass
abbrlink: 9908c040123123
date: 2020-04-28 14:51:33
description: 前端压缩图片进行优化 API图片压缩功能
---

### 前言

前端在日常的开发过程中，不免会产生大部分切图和 icon,但是每次处理这些事情都会比较繁琐，那怎么才能在这种情况下脱离出来呢？

<!--more-->

### 安装 tinypng-cli

> 安装 tinypng-cli 的好处是什么？方便一键处理大图片文件

#### 安装方式

```
cnpm install -g tinypng-cli
```

新建一个`.tinypng`文件，把注册好的 API KEY 放进去,然后保存好即可 每个月 `500`/`张`的次数，勉强能够开发使用

> tinypng for mac

```
vim ~/.tinypng
```

#### 使用方式

1.要收缩当前目录中的所有 PNG 图像，您可以运行以下命令之一-两者都完全相同。

> `tinypng . // tinypng`

2.要缩小当前目录和子目录中的所有 PNG 图片，使用-r 标记

> `tinypng -r`

3.要收缩特定目录中的所有 PNG 图像（assets/img 在此示例中），可以运行以下命令。单个文件同理

> `tinypng assets/img1`

单个文件

> `tinypng assets/img1/1.png`

多个目录使用多个

> `tinypng assets/img1 assets/img2 ···`

多个文件使用多个

> `tinypng assets/img1/1.png assets/img1/2.png ···`

要调整图像大小，请使用--width 和/或--height 标志。

```
tinypng assets/img/demo.png --width 123
tinypng assets/img/demo.png --height 123
tinypng assets/img/demo.png --width 123 --height 123
```
