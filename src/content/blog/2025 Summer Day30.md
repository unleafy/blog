---
title: '2025 Summer Day30'
description: '暑期集训 Day30'
date: '2025-08-11T03:22:39.534Z'
draft: false
tags:
  - 比赛
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---
# 2025 Summer Day30


**Content**：模拟赛
**Date**：2025.8.15

# Problem-A [图书配对](https://zhengruioi.com/problem/3268?cid=1960)

## 题目描述

给定 $n$ 本图书，定义 $merge(a_{i},a_{j})$ 表示 $a_i$, $a_{j}$ 直接拼接得到的结果，求满足 $merge(a_{i},a_{j}) \mid 9$ 的个数（一个数只能取一次）。

## 解题思路

我们可以运用小学奥数的知识，如果一个数能被 $9$ 整除，当且仅当这个数的各个数位之和能被 $9$ 整除，知道这个之后就是一个很经典的问题了。

提交记录：[Link](https://zhengruioi.com/submission/936081)

# Problem-B [菜品搭配](https://zhengruioi.com/problem/3269?cid=1960)

## 题目描述

给定数组 $a$，求 $\displaystyle \sum_{i=0}^{n-2} \sum_{j=i+1}^{n-1} \sum_{k=j+1}^{n} (a_{i} \oplus a_{j}) \times (a_{j} \oplus a_{k})$。

## 解题思路

由于是二进制意义下的异或运算，很容易想到拆位，计算每一位的贡献。而且求的是三元组 $(i,j,k)$，所以我们枚举 $j$ 的位置，考虑 $j$ 左右两边的贡献。

对于异或 **相同为 0，不同为 1** 的运算法则，我们可以发现，$i,k$ 对于 $j$ 的贡献是左右两边与 $j$ 不相同的位置的个数的乘积，所以我们对于每一个位置计算贡献即可。

涂胶记录：[Link](https://zhengruioi.com/submission/937158)

# Problem-C [课程推荐](https://zhengruioi.com/problem/3270?cid=1960)

## 题目大意

给你一个数组 $a$，其中 $a_i$ 表示到达 $i$ 这个位置后你可以到达 $[i+1,a_{i}]$ 中的任何一个位置，求 $\forall i,j$ 互相可达的最小步数之和。

## 解题思路 

赛时没有想到是 $dp$，直接写了个 Floyd (20pts) + 部分分(10pts)。

正解是定义 $d_{i,j}$ 表示从 $i$ 到 $j$ 需要的最少步数。容易发现：

+ 若 $j \in [i + 1, a_{i}]$，则 $d_{i,j}=1$。
+ 若 $j > a_{i}$，则 $d_{i,j} = d_{k,j} + 1$，其中 $k$ 表示 $\max_{t\in[i + 1, a_{i}]} a_t$ 所在的位置。

然后记 $\displaystyle f_{i} = \sum_{j=i}^{n} d_{i,j}$，则 $f_{i} = f_{p} + (n - i) - (a_{i} - p)$。答案即为 $\displaystyle \sum_{i=0}^{n} f_{i}$。

---

终于结束啦喵~