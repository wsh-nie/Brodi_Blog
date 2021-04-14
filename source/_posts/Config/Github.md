---
layout: post
title: win10 github多账号配置
draft: false
description: 配置生产环境
tags:
  - win10
categories:
  - Config
mathjax: true
abbrlink: e3dc2e3b
date: 2019-08-01 21:16:35
---


# win10 github多账号设置

1. 生成对应公私钥
```powershell
ssh-keygen -t rsa -C githubEmail
```
2. 设置对应公私钥文件名，因为是多账号所以必须加以区分，文件必须放置在`git`默认访问的`.ssh`目录下  
   
3. 把`*.pub`上传到对应`github`账号中

4. 在.ssh目录下创建config文本文件并进行相关配置
```config
Host <alias>
HostName <host name>
IdentityFile <private key's Absolute path>
User <user name>
```   

以下是两个账号的记录

```config
#Ilooker
Host Ilooker            
HostName github.com
IdentityFile C:\\Users\\wshni\\.ssh\\id_rsa_Ilooker
PreferredAuthentications publickey
User Ilooker

#wsh-nie
Host wsh-nie
HostName github.com
IdentityFile C:\\Users\\wshni\\.ssh\\id_rsa_wsh-nie
PreferredAuthentications publickey
User wsh-nie
```

5. 注意在clone的时候是需要根据前面设置的alias进行修改命令行
```git
git clone git@<ailas>:<user name>/<respority name>.git
```