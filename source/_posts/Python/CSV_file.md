---
layout: post
title: python 读写csv文件
description: 使用csv文件遇到的一些问题
abbrlink: a378bd8e
date: 2021-05-13 15:07:57
tags:
 - python
 - file process
catagories:
 - programmer
---

Python有读取cvs文件的专门的包[`csv`](https://docs.python.org/zh-cn/3/library/csv.html)

#### 读文件

```python
import csv

with open(file_path, 'r') as f:
    reader = csv.reader(f) # all infomation saved in the list "reader"
    for read_row in reader:
        # your process
        # type(read_row) is list
```

#### 写文件

```python
import csv
datas = []
with open(file_path, 'w') as f:
    writer = csv.writer(f)
    for data in datas:
        writer.writerow(data)
```

会发现每一行都会多出一列空白，可用下面

```python
with open(file_path, 'w', newline="") as f:
    writer = csv.writer(f)
    for data in datas:
        writer.writerow(data)
```

## 注意

惨痛经历！！！

用Excel打开csv文件时，千万别用填充颜色和字体颜色来作为标注，这些信息是不会被csv文件存储的，所以只要关了文件就会丢失的。