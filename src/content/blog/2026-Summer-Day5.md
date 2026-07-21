---
title: '2026 Summer Day5'
description: ''
date: '2026-07-21T12:51:03.815Z'
draft: false
showHeroImage: false
tags: ['树剖']
categories: []
series: []
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day5

## 课堂内容

### 重链剖分

略，没什么好说的喵。

### 长链剖分

长链剖分是按照每个节点到叶子节点的深度来判定重儿子的一种树剖方式，这种树剖方式主要用于对于一些与**深度有关** DP 的转移方程的优化的。通过动态分配来降低时间和空间复杂度至线性。

## 练习

### P2486 [SDOI2011] 染色

[题目链接](https://www.luogu.com.cn/problem/P2486)

这道题就是重链剖分，然后用线段树维护重链的颜色连续段，注意这道题询问的时候不可以交换 $u, v$，因为最后合并的时候和这个顺序有关，在线段树查询的时候 $u, v$ 的序列在路径上不是连续的，所以需要分别统计。

[code](https://www.luogu.com.cn/record/287566986)

### CF1009F Dominant Indices

[题目链接](https://codeforces.com/problemset/problem/1009/F)

这道题显然是要 dp 的~~废话~~。

我们定义 $f_{u, d}$ 表示在 $v$ 的子树内到节点 $u$ 的距离为 $d$ 的 $v$ 的数量，转移就是：

$$
f_{u, i} = \sum f_{v, i - 1};
$$

这样做的复杂度是 $O(n^2)$ 的，但是发现方程和深度有关，于是我们可以考虑使用长链剖分优化。

我们通过动态分配内存的方式，每次将重儿子的地址由 $u$ 的地址向后移动一位，这样 $son_u$ 到 $u$ 的转移就可以省略了。

[code](https://codeforces.com/contest/1009/submission/383696874)
