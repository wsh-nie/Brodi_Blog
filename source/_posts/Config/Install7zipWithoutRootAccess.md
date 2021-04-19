---
layout: post
title: Linux下非管理员权限用户下载并安装7zip
draft: false
description: 生产环境配置
tags:
  - Linux
categories:
  - Config
mathjax: true
abbrlink: fe6e3ed6
date: 2021-04-19 10:56:26
---


需求：下载数据集需要使用到7zip进行解压缩，但无管理员权限所以无法直接使用命令行安装和下载，需要在非管理员权限下下载并安装7zip

## 下载

```shell
wget http://nchc.dl.sourceforge.net/project/p7zip/p7zip/9.20.1/p7zip_9.20.1_src_all.tar.bz2
tar -jxvf p7zip_9.20.1_src_all.tar.bz2
```

## 编译

```shell
cd p7zip_9.20.1
make
make install
```

报错：`Permission denied`

## 修改有权限路径

* 修改makefile和install.sh

```
DEST_HOME=/home/[username]/local #即改为自己有操作权限的路径即可
```

* 安装

```
make install
```

```
./install.sh [DEST_HOME]/bin [DEST_HOME]/lib/p7zip [DEST_HOME]/man /[DEST_HOME]/share/doc/p7zip 
- installing [DEST_HOME]/bin/7za
- installing [DEST_HOME]/man/man1/7z.1
- installing [DEST_HOME]/man/man1/7za.1
- installing [DEST_HOME]/man/man1/7zr.1
- installing [DEST_HOME]/share/doc/p7zip/README
- installing [DEST_HOME]/share/doc/p7zip/ChangeLog
- installing HTML help in [DEST_HOME]/share/doc/p7zip/DOCS
```

如上显示即显示安装成功

## 添加环境变量

```
vi ~/.bashrc
[insert] export PATH="[DEST_HOME]/bin:$PATH" # 这里[DEST_HOME]得改成你之前配置的路径
source /etc/profile
```

输入`7za`即显示已可用该命令


## 参考

1. [安装7zip](https://www.cnblogs.com/xiao-apple36/p/9264875.html)
2. [环境变量](https://www.cnblogs.com/aaronLinux/p/5837702.html)