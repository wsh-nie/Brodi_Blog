---
layout: post
title: Linux 建立文件夹之间的软连接
draft: false
description: 大文件和主程序不在一个文件夹下
tags:
  - Linux
categories:
  - Config
abbrlink: '637e1180'
date: 2021-04-19 15:58:25
---

目标：数据集太大，不直接放在服务器用户账号下，存放于`/media/workspace/[username]`下，主程序在`/home/[username]/`下面，运行程序时需要建立软连接


## 硬链接与软连接

Linux有两种指向文件的方式，分为：Hard Link【硬链接】，Symbolic Link【符号链接，又称软连接】。区别在于删除文件时，会删除相应的软连接而存在的硬链接仍能够保存文件。

文件存在硬盘中，文件名可以理解为指向文件开头地址的指针，为文件创建一个硬链接即创建一个指针指向该文件，当删除文件时仅删除原有指向文件开头的指针，硬链接仍指向文件，故文件仍可通过硬链接被访问。而软连接可以理解为指向原有指针的指针，在删除原有文件时会优先删除指向其的指针，即软连接。也可以将软连接理解为windows下的快捷方式。

## 软连接操作

* 创建软连接
```
ln -s [/SymbolicLinkFile | /SymbolicLinkFile/] [/TargetFile | /TargetFile/]
```

```
ls -il #可查看该文件夹的软连接
```

* 删除软连接

```
rm –rf [/SymbolicLinkFile]->[/TargetFile]
```

* 修改软连接

```
ln -snf [/SymbolicLinkFile] [/TargetFile]
```

## 参考

1. [建立软连接](https://kukksaku.github.io/2019/02/02/Ubuntu%E6%96%87%E4%BB%B6%E5%A4%B9%E5%BB%BA%E7%AB%8B%E8%BD%AF%E9%93%BE%E6%8E%A5/)
