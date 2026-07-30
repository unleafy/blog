---
title: "2026 Summer Day12"
description: ""
date: "2026-07-28T14:34:52.958Z"
draft: false
showHeroImage: false
tags: ["模拟赛"]
categories: ["记录"]
series: ["2026 Summer"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day12

## T3 删除滚木

[题目链接](https://zhengruioi.com/problem/3575)

这道题在考场上第一 ~~直觉~~ 就把二分答案给排出了，因为觉得删除一个位置上的数会导致答案变大或者变小，显然是不具有单调性的。但是这个分析是对于策略而言的，而不是对于二分答案的正确性而言的。

我们考虑二分答案 $mid$，则要让这个答案合法，需要

$$
|a_{p_{i + 1}} - a_{p_{i}}| \le (p_{i + 1} - p_i) \times mid
$$

然后我们把绝对值拆开，就得到了：

$$
\begin{cases}
a_{p_{i+1}} - mid \times p_{i+1} \le a_{p_i} - p_i \times mid \\
a_{p_{i}} - mid \times p_{i} \le a_{p_{i+1}} - p_{i+1} \times mid \\
\end{cases}
$$

这个时候我们其实就可以用三维偏序做了，但是还是很麻烦，看着就很难写。我们令 $C_i = a_i - mid \times i, D_i = a_i + mid \times i$，那么就有 $D_{p_i} - C_{p_i} \le D_{p_{i+1}} - C_{p_{i+1}}$ 所以我们只要满足前两个条件，第三个条件就会自然满足。然后我们 check 的时候只需要做一个最长上升子序列即可。复杂度 $O(n \log n)$。

[code](https://zhengruioi.com/submission/1076735)

## T4 午安。

依旧是诡异的题目。要你求：

$$
\sum_{i = 1}^m \sum_{j = i}^n \max_{k=i}^j a_k \operatorname{op} \min_{k=i}^j a_k
$$

对 $998244353$ 取模的结果，其中 $op$ 为加、减、乘、除、取模 ~~大毒瘤~~。

对于加减直接吧 $min,max$ 分开处理即可，复杂度 $O(n)$，可以用单调栈处理合法区间。

然后考虑乘法，我们可以使用分治来计算跨国分治中点 $mid$ 的区间对答案的贡献。这时候需要使用双指针维护当前区间的前缀、后缀 $min,max$，然后分类讨论即可。复杂度 $O(n \log n)$

出发和取模本质是一样的，我们可以考虑枚举当前这个数以及它的商，这个复杂度是调和级数 $O(n \log n)$ 的，然后就二分符合要求的区间计算答案即可。复杂度 $O(n \log^2 n)$。

[code](https://zhengruioi.com/submission/1077172)
