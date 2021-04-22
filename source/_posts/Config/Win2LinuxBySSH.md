---
layout: post
title: 本地windows通过跳板机连接Linux服务器
draft: false
description: 配置生产环境
tags:
  - Linux
  - DL
categories:
  - Config
mathjax: true
abbrlink: 27a9e9b0
date: 2021-04-14 16:33:20
---

目的：本地主机连接服务器，能够通过vscode对服务器上的代码进行debug，不必每次vim且无法可视化
环境：本地操作系统为win10[Client]，服务器[server]为linux，中间还需要经过一次跳板机[jumper_server]的跳转
连接方式：SSH

## Client上安装OpenSSH

win+R 打开cmd，输入指令`where ssh`，查看是否安装OpenSSH

* 情景一：未安装，参考[官方文档](https://docs.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_install_firstuse)安装OpenSSH
* 情景二：只显示一个SSH路径，无碍
* 情景三：由于之前装过git，显示超过一个ssh路径，同样参考上面的官方文档卸载ssh的客户端，注意，只要卸载客户端不需要卸载服务端，此外要保证git自带的ssh加到了环境变量Path中。我是用[scoop](/posts/61e990a2/)安装的git，所以不用管这个。至于为什么卸载win10 自带的ssh，就是自己试出来的，骂一句windows傻逼就行，毕竟你还要用git【今天win10更新了，然后发现连接不了服务器，一看又给我装上了OpenSSH客户端，又得自己卸载】

## 配置SSH公钥

前提，你应该可以通过PowerShell或者git bash等其他路径登录跳板机和服务器，并有权限读写你的文件

第一步：在本地使用`ssh-keygen`生成一对公私钥，可以使用git bash, PowerShell（管理员权限） etc.
```git
$ ssh-keygen -t rsa -b 4096
```
一直按enter即可，当然如果后续需要或者已经配置好github的可以参考[github](/posts/e3dc2e3b)，对不同的公私钥对进行命名区别

在`C:\Users\[YOUR_WINDOWS_NAME]\.ssh`文件夹下面有了两个文件`id_rsa`和`id_rsa.pub`

用记事本打开`id_rsa.pub`，复制内容，上传到jumper_server和server的`~/.ssh/authorized_keys`文件中，如果没有该路径，请依次创建文件夹和文件，注意，`authorized_keys`是没有后缀的文件，而非目录

## 配置本地config

打开`C:\Users\[YOUR_WINDOWS_NAME]\.ssh`文件夹，并创建`config`文件，注意是文件不是文件夹，将以下配置信息按照你的信息进行修改并保存

```config
Host NAME_YOUR_JUMPER_SERVER                                            # 为了区分，你自己给跳板机命名，尽量按照C语言的变量命名规则
    HostName 66.xxx.xxx.xxx                                             # 跳转机的ip，可以是域名
    Port 39002                                                          # 对应的端口
    User YOUR_USER_NAME                                                 # 你在跳板机上的的用户名
    IdentityFile ~/.ssh/id_rsa                                          # 就是上一步生成的私钥位置
    
Host NAME_YOUR_SERVER
	HostName 192.168.1.xx
	Port 22                                                             # 这里默认22号端口
	User YOUR_USER_NAME                                                 # 你在服务器上的用户名
	ProxyCommand ssh YOUR_USER_NAME@NAME_YOUR_JUMPER_SERVER -W %h:%p
	IdentityFile ~/.ssh/id_rsa
```

然后再git bash或者PowerShell上使用`ssh NAME_YOUR_XXXX`看看能否连接到跳板机和服务器，首次登录是需要输入登录密码的。如果登录不了，检查以上步骤。

记录浪费了自己五个多小时的错误配置信息，`YOUR_USER_NAME@NAME_YOUR_JUMPER_SERVER`，用跳板机的ip替换了`NAME_YOUR_JUMPER_SERVER`，ssh的时候一直报`ssh：connect to host 66.xx.xx.xx port 22: Connection timed out`。google的结果就是被墙了，其实想来也对，按照我的写法连接的是服务器的22号端口，但服务器开放的是39002号端口，复盘结果就是应该看到跳板机的22号端口就该想到连接错误。

## 使用VSCode连接服务器

VSCode下载Remote SSH插件，发现左边出现了新的功能图标

点击`settings`，检索`Remote-SSH`将之前配置的config路径加到config file中去

点击新的功能图标，发现解析到配置文件里面的连个Host，点击连接即可
