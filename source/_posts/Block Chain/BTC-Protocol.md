---
title: "BTC-相关协议"
date: 2020-05-23 14:40:39
draft: false
description: "比特币的相关协议"
tags: 
- courses notes
- block chain
categories: 
- block chain
mathjax: true
---

# 交易

## double spending attack

数字货币需要解决的第一个问题就是double spending attack。

## block header

包含区块的一些宏观信息

- version 指明用的是比特币哪个版本的协议
- hash of previous block header 区块链中指向前一区块的指针
- Merkel root hash 整个merkel tree的根哈希值
- target 挖矿的难度
- nonce 随机数

仅含block header的是轻结点，系统中大多都是轻结点，不参与区块链的构造与维护。

## block body

- transaction list 交易列表

全节点参与区块链的构造与维护。

## distributed consensus

分布式共识

distributed hash table
分布式哈希表

FLP不可能结论(FLP impossibility result):
在异步系统(asynchronous)里，网络传输时延没有上限，即使只有一个成员是faulty也不可能取得共识。

CAP 定理(Consistency-Availability-Partition tolerance Theorem)：
三个性质中最多只能满足两个。

分布式共识的著名协议：Paxos，该协议能保证consistency

## Consensus in BitCoin

解决恶意结点。

考虑投票机制：

首先需要决定membership，即谁有投票权，否则恶意账号可以产生超过半数的账号实施sybil attack。

比特币系统中，按照计算力来投票。

每个结点都可以在本地组装出候选区块，把它认为合法的交易放到区块里，然后就开始尝试各种nonce值，当某个节点找到符合要求的nonce则获得记账权。所谓记账权即往比特币这个去中心化账本里写入下一个区块的权利。

为避免forking attack，即通过往区块链中的某个区块插入分支来回滚已经发生过的交易，区块链中要求插入最长合法链。

当差不多同时两个结点获得记账权，等长的临时性分叉会存在，在之后写入的竞争后，输掉的区块成为了orphan block。

## block reward

获得记账权的结点在发布的区块里可有一个特殊的交易，即铸币交易，可以发布一定的比特币。

coinbase transaction是比特币系统当中发行新的比特币的唯一方法，其他所有的交易都只不过是把已有的比特币从一个账户转移到另外一个账户。