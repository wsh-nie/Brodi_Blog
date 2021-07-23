---
layout: post
title: Linux下非管理员权限用户Anaconda的下载、安装以及使用
draft: false
description: 生产环境配置
tags:
  - Linux
categories:
  - Config
mathjax: true
abbrlink: 
date: 2021-07-22 14:26:38
---

# 下载

官网：https://www.anaconda.com/products/individual#linux

$F_{12}$ 找到下载连接：https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh

# 安装

安装：`bash Anaconda3-2021.05-Linux-x86_64.sh`

然后一直按`Enter`进行浏览协议，大概是70词，放心，一直按，后面会让你输入`yes`同意协议

然后要你输入安装路径，按`Enter`默认`/home/username/anaconda3`

配置环境变量：

```shell
echo 'export PATH="/home/xxx/anaconda3/bin:$PATH"'>>~/.bashrc
source ~/.bashrc
```

# 使用

- 创建并激活一个环境
```anaconda
conda create --name your_env_name python=3.8
```

- 查看所有环境
```anaconda
conda env list
```

- 进入环境
```
source activate your_env_name
```

- 退出环境
```
conda deactivate
```

- 删除环境
```
conda remove --name your_env_name --all
```
- 复制环镜
```
conda create -n your_env_name --clone your_new_env_name
```

- 检查环境
```
conda info --envs
```

- 分享环境
```
conda your_env_name > enviroment.yml
```

# 根据`requirements.txt`配置环境

```
pip install -r requirements.txt
```

看Video Swin Transfomer的操作`requirements.txt`
```
-r requirements/build.txt
-r requirements/optional.txt
-r requirements/runtime.txt
-r requirements/tests.txt
```

然后再在各文件中记录相应配置