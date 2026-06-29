---
title: '2025 Summer Day22'
description: '暑期集训 Day22'
date: '2025-08-03T03:22:39.534Z'
draft: false
tags:
  - 字符串
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day22


# 课堂内容

## 后缀数组

后缀数组主要指两个数组 $sa$ 和 $rank$：
+ $sa_i$ 表示所有后缀中按字典序大小从小到大排序后排名为 $i$ 的后缀的起始位置。
+ $rank_i$ 表示以 $i$ 为起始位置的后缀的排名。

其中这两个数组有如下性质：

$$
sa_{rank_i} = rank_{sa_i} = i
$$

可以使用倍增 + 基数排序的方式求解 $sa$ 和 $rank$ 数组，复杂度 $O(n \log n)$。

除此之外，还有一个 $height$ 数组表示 $sa_i$ 和 $sa_{i-1}$ 两个后缀的 LCP (Longest Common Prefix) 的长度。

模版题：[Luogu-P10469](https://www.luogu.com.cn/problem/P10469) 后缀数组
提交记录：[Link](https://www.luogu.com.cn/record/197038066)

# 例题

## [Luogu-P3809](https://www.luogu.com.cn/problem/P3809) 后缀排序

### 题目大意

给定一个长度为 $n$ 的仅由小写字母组成的字符串，将其所有后缀按字典序从小到大排序后，求排名为 $i$ 的后缀的起始位置。

### 思路

就是后缀数组的简单应用，求出 $sa$ 数组后输出即可。注意下标的问题。

提交记录：[Link](https://www.luogu.com.cn/record/229394389)