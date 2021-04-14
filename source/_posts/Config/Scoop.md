---
layout: post
title: Scoop
draft: false
description: 配置生产环境
tags:
  - win10
categories:
  - Config
mathjax: true
abbrlink: 61e990a2
date: 2019-09-01 09:28:45
---

# Scoop

## 为什么使用[scoop](https://scoop.sh/)

在公司开发中，因为配置`hogo`环境，发现了这款神器，当时就决定自己再次重装系统的时候一定要使用这个好东西。给电脑换上固态后，装好`win10`就用来练手了。

开发过程中最麻烦的事就是配置环境了，配环境如果能够一帆风顺那倒无话可说，可总是会出现千奇百怪的错误的呀，而且安装程序又会产生各种垃圾并且会污染系统环境变量，卸载又难以彻底清理干净。`Scoop`作为史上最强大的`Windows`命令行包管理工具，能够很好的解决问题。

我选择`scoop`主要在于`scoop`会自动设置环境变量，他这个设置环境变量并不是将`path`添加到系统环境变量中，此外`scoop`也会管理程序依赖，我们可以很好处理配置开发环境时面临的各种问题了。




## 安装`scoop`

安装`scoop`需要使用`powershell`

```powershell
#打开powershell：win+r，输入powershell

#更改策略，输入A选择“全是”
Set-ExecutionPolicy RemoteSigned -scope CurrentUser

#命令行安装scoop 
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```

安装信息如下：

```powershell
Initializing...
Downloading scoop...
Extracting...
Creating shim...
Downloading main bucket...
Extracting...
Adding ~\scoop\shims to your path.
'lastupdate' has been set to '2019-09-05T17:35:29.5012454+08:00'
Scoop was installed successfully!
Type 'scoop help' for instructions.
```

安装完成之后，输入`scoop help`查看可用指令，常见指令如下：

```powershell
#安装某个软件
scoop install <software name>

#搜索某软件，一般先搜索software name，在执行install操作
scoop search <software name>

#卸载软件
scoop uninstall <software name>

#打开软件官网
scoop home <software name>

#查看软件安装信息
scoop info <software name>

#查看软件执行命令位置
scoop which <software name>

#查看软件当前状态，是否有更新等信息
scoop status <software name>

#更新软件
scoop update <software name>

#更新 scoop，一般会在install的时候自动update
scoop update

#查看安装软件列表
scoop list
```

## 安装软件举例——jdk

养成良好的习惯，我们先执行`search`命令:

```powershell
scoop search jdk
```

我们将得到如下结果:
```powershell
Results from other known buckets...
(add them using 'scoop bucket add <name>')
'java' bucket:
    adoptopenjdk-hotspot-jre (12.0.2-10)
    adoptopenjdk-hotspot (12.0.2-10)
    adoptopenjdk-lts-hotspot-jre (11.0.4-11)
    adoptopenjdk-lts-hotspot (11.0.4-11)
    adoptopenjdk-lts-openj9-jre (11.0.4-11-0.15.1)
    adoptopenjdk-lts-openj9 (11.0.4-11-0.15.1)
    adoptopenjdk-openj9-jre (12.0.2-10-0.15.1)
    adoptopenjdk-openj9 (12.0.2-10-0.15.1)
    adoptopenjdk-upstream (11.0.3-7)
    ojdkbuild-full (12.0.1.12-1)
    ojdkbuild (12.0.1.12-1)
    ojdkbuild10-full (10.0.2-1.b13)
    ojdkbuild10 (10.0.2-1.b13)
    ojdkbuild11-full (11.0.3.7-1)
    ojdkbuild11 (11.0.3.7-1)
    ojdkbuild12-full (12.0.1.12-1)
    ojdkbuild12 (12.0.1.12-1)
    ojdkbuild8-full (1.8.0.212-1.b04)
    ojdkbuild8 (1.8.0.212-1.b04)
    ojdkbuild9-full (9.0.4-1.b11)
    ojdkbuild9 (9.0.4-1.b11)
    openjdk (12.0.2-10)
    openjdk10 (10.0.2-13)
    openjdk11 (11.0.2-9)
    openjdk12 (12.0.2-10)
    openjdk13 (13-33)
    openjdk14 (14-12-ea)
    openjdk7-unofficial (7u80-b32)
    openjdk9 (9.0.4-12)
    oraclejdk (12.0.2-10)
    oraclejdk12 (12.0.2-10)
```
选择我们所需要的软件进行安装(吐槽一下居然没有`jdk8`，官网也没了下载链接)：

```powershell
scoop install openjdk11
```

出现如下：

```powershell
Couldn't find manifest for 'openjdk11'.
```
确认没有拼写弱智错误，仔细分析发现`search`命令的时候显示的结果第一条`'java' bucket:`，使用`scoop bucket known`查看，结果如下：

```powershell
main
extras
versions
nightlies
nirsoft
php
nerd-fonts
nonportable
java
games
jetbrains
```

查看文档之后发现，原来bucket的概念就相当于软件源，软件必须放置在对应的bucket里面。

我们再来查看一下`default bucket`:

```powershell
scoop bucket list
```

显示只有一个`main`，所以先建立`jdk`所需对应的源：

```powershell
scoop bucket add java
```

出现如下结果：

```powershell
Git is required for buckets. Run 'scoop install git' and try again.
```
所以啊，先安装`git`吧，坚持良好习惯：

```powershell
scoop search git
```

结果如下：

```powershell
'main' bucket:
    git-annex (7.20190731)
    git-chglog (0.8.0)
    git-crypt (0.6.0-701fb8e)
    git-interactive-rebase-tool (1.1.0)
    git-istage (0.2.61)
    git-lfs (2.8.0)
    git-sizer (1.3.0)
    git-tfs (0.30)
    git-town (7.2.1)
    git-up (1.6.1)
    git-with-openssh (2.23.0.windows.1)
    git (2.23.0.windows.1)
    gitea (1.9.2)
    gitignore (0.2018.07.25)
    gitkube (0.3.0)
    gitlab-runner (12.2.0)
    gitomatic (0.2)
    gitversion (5.0.1)
    llvm (8.0.1) --> includes 'git-clang-format'
    mingit-busybox (2.23.0.windows.1)
    mingit (2.23.0.windows.1)
    psgithub (2017.01.22)
    psutils (0.2018.08.04) --> includes 'gitignore.ps1'
```

git是在main bucket里面的，所以现在可以安装git了，安装成功之后，提示新的任务：
```powershell
'git' (2.23.0.windows.1) was installed successfully!
Notes
-----
To get Git to recognise OpenSSH, you will need to run

scoop install openssh
[environment]::setenvironmentvariable('GIT_SSH', (resolve-path (scoop which ssh)), 'USER')

and then restart powershell.
```
能怎么办，按照命令执行呗。

好了，重启`powershell`之后我们应该创立`java bucket`了，然后就可以下载`jdk11`了。

`scoop`会帮助我们管理程序之间的依赖，在安装其他软件的时候自动先`Uploading scoop`或者安装必要的一些前置软件，比如`7zip`。

## 使用中碰到的问题

最大的问题当然是，下载中断了怎么办，又是外网。

想我第一次使用`scoop install hugo`因为网络连接超时准备重新安装时候出现故障，不看文档直接跑去`github`提交[issue](https://github.com/lukesampson/scoop/issues/3560)。真心感谢那些能够积极维护`community`的大佬。

其实，只要牢记一点，不管你`install`或者`bucket add`这些操作执行成功与否，他们都已经在`scoop`里面占好坑了，一旦下载和安装发生了失败，可以执行list命令查看，然后先将被破坏的文件删掉(`uninstall <software name>/rm bucketname`)再重新执行。

那么问题来了，如果是最开始下载`scoop`的时候出现故障呢？

要去`C:\Users\<username>`先把`scoop`文件夹给删除掉再重新安装。

## 感受

一字蔽之：爽。

推荐一个好用的软件：

```
scoop install aria2
```

你会发现下载速度起飞了。

2019.9.3

