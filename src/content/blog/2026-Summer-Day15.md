---
title: "2026 Summer Day15"
description: ""
date: "2026-07-31T10:36:02.814Z"
draft: false
showHeroImage: false
tags: ["数学"]
categories: ["记录"]
series: ["2026 Summer"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day15

## P12639 [UOI 2020] Topological Sorting of a Tree

[题目链接](https://www.luogu.com.cn/problem/P12639)

这道题课上讲的容斥做法没怎么听懂，看题解发现 *DP* 好像更好理解。

我们令 $dp_{i, j}$ 表示当前以 $i$ 为根的子树内 $A_i$ 的排名为 $j$，转移就是枚举儿子 $v$ 以及当前小于（大于） $A_i$ 的数有多少来自于 $v$ 的子树。

[code](https://www.luogu.com.cn/record/289806911)

## P6086 【模板】Prüfer（Prufer）序列

[题目链接](https://www.luogu.com.cn/problem/P6086)

板子题，不讲。

注意一下 $n \le 5 \times 10^6$，所以 $i \times a_i$ 会爆 int。

[code](https://www.luogu.com.cn/record/289863700)

## 文艺计算姬

[题目链接](https://vjudge.net/contest/836962#problem/E)

考虑左侧点是长度为 $n-1$ 的、值域为 $[1, m]$ 的序列，右侧是长度为 $m-1$、值域为 $[1, n]$ 的序列，又因为是完全二分图，所以答案即为 $m^{n-1}n^{m-1}$。

[code](https://vjudge.net/solution/71535393)
