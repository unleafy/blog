---
title: '2025 Summer Day29'
description: '暑期集训 Day29'
date: '2025-08-10T03:22:39.534Z'
draft: false
tags:
  - 杂题
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day29


**Content**：杂题
**Date**：2025.8.14


# [CodeForces-1870E](https://www.luogu.com.cn/problem/CF1870E) Another MEX Problem

## 题目大意

给你一个数组 $a$，你可以选择任意互不相交的子数组，先计算每一个子数组的 MEX 值，然后将这些 MEX 值的异或和作为这个方案的价值，求你可以获得的最大价值。

## 解题思路

首先我们肯定可以暴力 DP，定义 $dp_{i,j}$ 表示前 $i$ 个数能否凑出价值为 $j$ 的方案，转移显然是 $O(n^3)$ 的。

考虑如何优化。由于 MEX 的性质，可以发现从 $i$ 往后的一段区间内 MEX 值是单调不降的，所以其中一定有大量相等的段。这些段本质是一样的，所以我们只需要考虑那些前后不同的位置即可，这些符合条件的转移区间至多有 $2n$ 个，所以我们就得到了复杂度为 $O(n^2)$ 得做法。

提交记录：[Link](https://vjudge.net/solution/62866466)

# [Luogu-P12426](https://www.luogu.com.cn/problem/P12426) BOI acroym

## 题目大意

已知一个由 B, O, I 组成得字符串每个区间得众数得出现次数，且 B 为全局众数，求原串种 B 出现的位置。

## 解题思路

首先我们可以找到第一个和最后一个出现的 B 的位置。
+ 左侧最后一个满足 $m_{1,n} = m_{l,n}$ 的位置 $l$。
+ 右侧最后一个满足 $m_{1,n} = m_{1,r}$ 的位置 $r$。

然后我们考虑根据已知信息推出区间 $(l,r)$ 内的 B 的位置。主要有以下三种情况 ($cur$ 表示当前已知的 B 的数量)：

1. 若 $m_{l,i - 1} = cur$ 且 $m_{l + 1, i - 1} = cur - 1$ 且 $m_{l,i} = m_{l,i - 1} + 1$，则位置 $i$ 为 B。
2. 若 $m_{i,r} = m_{l,r} - cur$ 且 $m_{i, r - 1} = m_{l,r} - cur - 1$ 且 $m_{i,r} = m_{i + 1,r} + 1$，则位置 $i$ 为 B。
3. 若 $m_{l + 1,i} = m_{l,i-1}$ 且 $m_{i,r} = m_{i + 1,r - 1}$，则位置 $i$ 为 B。

提交记录：[Link](https://www.luogu.com.cn/record/230905777)

# [Luogu-P8386](https://www.luogu.com.cn/problem/P8386) [PA 2021] Od deski do deski

## 题目描述

给定 $n$，$m$，求满足以下限制的长度为 $n$ 的序列数目：

1. 每个元素在 $[1,m]$ 之间；
2. 一次操作定义为删除一个长度至少为 $2$ 且区间两端相等的区间，该序列需要在若干次操作内被删空。

答案对 $10^9+7$ 取模。

## 解题思路

可以发现最后的答案一定是由 $a \dots\dots\dots a$ 这样的字符串拼接成的。所以我们定义 $f_{i,j,0 / 1}$ 表示前 $i$ 个位置包含 $j$ 个不同的数且符合/不符合条件的方案数，转移显然。

$$
\begin{align}
f_{i,j,1} \times j &\to f_{i+1,j,1} \\
f_{i,j,1} \times (m - j) &\to f_{i+1,j + 1,0} \\
f_{i,j,0} \times j &\to f_{i+1,j,1} \\
f_{i,j,0} \times (m - j) &\to f_{i+1,j,0}
\end{align}
$$

答案即为 $\displaystyle \sum_{i=0}^{n} f_{n,i,1}$，注意取模。

提交记录：[Link](https://www.luogu.com.cn/record/230890956)