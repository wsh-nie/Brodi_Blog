---
title: "BTC-数据结构"
date: 2020-05-23 09:42:39
draft: false
description: "比特币的数据结构"
tags: 
- courses notes
- block chain
categories: 
- block chain
mathjax: true
---

# Hash Pointers

哈希指针$H$指向结构体的位置，还能检测结构体的内容是否被篡改。

如下图所示，每个区块中包含前继区块的哈希值，而且这个值也作为计算自身哈希值的一部分。

当红色区块链的内容被篡改，将导致其哈希值被改变，进而导致其所有后继区块里面的哈希值的改变。

![图1](/images/Block_Chain_BTC_DS_1.png)

故使用该数据结构，只要保留最后的哈希值就可以检测出整个区块链中任何部位的修改。

在此基础上，比特币的有些结点就无需保存整条区块链的内容，只要保存最近的区块就可以。可以向其他结点申请未保存的区块，通过最前的区块中的哈希指针就能判断结点是否区块链上的。

区块链中分为两种结点：全节点(full node)和轻结点(light node)。

在全节点中，包含block header和block body两个部分，body里面保存的是交易列表，header里面包含较多信息，比如父区块的hash pointer，时间戳，挖矿的难易程度等。而轻结点中只包含block header。

# Merkle Tree

与binary tree的区别就是使用哈希指针代替了普通指针。

如图2，通过根哈希值就能检测出整个树中任何部位的修改。
![图2](/images/Block_Chain_BTC_DS_2.png)

## Merkle proof

用户系统中基本只保存轻结点(根哈希值)，那么该如何验证交易确实写入区块中(proof of membership)？

如下图中，假设某轻结点想知道黄色交易是否发生在该merkle tree里面，轻结点未保存交易链表，也没有交易的具体内容，只有一个根哈希值。

该轻结点向某个全节点发出请求，请求一个能够证明黄色结点交易被包含在这棵merkle tree里面的merkle proof。全节点接收到请求后把图中红色的三个结点的哈希值发给轻结点即可。

有个这些哈希值，轻结点可以在本地计算出图中标为绿色的哈希值。然后将计算出的根哈希值与merkle tree的哈希值进行对比，即可判断。

![图3](/images/Block_Chain_BTC_DS_3.png)

那么是否可以证明交易不在该区块中(proof of non-membership)？

蛮力法，将整棵树都告知轻结点，则可知交易不在该merkle tree中。

对所有的交易进行一次hash，然后按照hash值进行一个排序，然后根据要证明的交易的哈希值，使用二分法即可。

对交易哈希值进行排序的树结构称之为sorted merkle tree，比特币中没有使用这种技术，因为比特币不需要证伪。