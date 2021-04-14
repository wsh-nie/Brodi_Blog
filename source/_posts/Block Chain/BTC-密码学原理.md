---
title: BTC-密码学原理
draft: false
description: 比特币的相关密码学原理
tags:
  - courses notes
  - block chain
categories:
  - block chain
mathjax: true
abbrlink: 9ec7f0ee
date: 2020-05-23 08:37:39
---

比特币又称加密货币(`crypto-currency`)，实际上比特币的每个操作都是公开的。

# 密码学原理——哈希

比特币使用的哈希函数是$SHA-256$，Secure Hash Algorithm。

## collision resistance

抗碰撞性质的存在，使得无法人为的制造哈希碰撞。即对于一个信息$m$，无法人为的构造出$m'$，使得$H(m') = H(m)$。

故可以使用`collision resistance`来检测对`message`的篡改。

需要说明的是，哈希函数的抗碰撞性质是无法被证明的，它只是实践中的经验。同样的，也不存在绝对无法制造碰撞的哈希函数，比如$MD5$也是可以制造出哈希碰撞的。

## hiding

指的是哈希函数的计算过程是单向的，且不可逆的。该性质的前提是输入空间足够大，避免蛮力输入获得哈希值。

哈希的`hiding`性可结合`collision resistance`来实现`digital commitment`(`digital equivalent of a sealed envelope`)。

一般情况下，预测的原值公布可能会导致对预测结果的影响，所以可以通过公布预测的哈希值，等结果出现以后，通过对比原值经哈希映射后的值来判断预测的准确性。

## puzzle friendly

哈希值的计算事先是未知的，故需要产生特定格式的输出，只能使用暴力法，所以在挖矿中可用作工作的证明(`proof of work`)。

# 密码学原理——签名

## asymmetric encryptiion algorithm

去中心化开户，自己决定开户，无中心机构，只需创建公钥(public key)和私钥(private key)对。

别人使用公钥给你发送信息，然后经过本地的私钥进行解密。

当使用比特币进行交易时，使用本地的私钥进行签名，然后其他人可以使用公钥进行验证。

需要的是使用好的随机元来产生随机的公私钥对。