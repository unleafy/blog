---
title: "2026 Summer Day10"
description: ""
date: "2026-07-26T12:48:41.696Z"
draft: false
showHeroImage: false
tags: ["DP 优化"]
btcategories: ["记录"]
series: ["2026 Summer"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day10

## P3195 [HNOI2008] 玩具装箱

[题目链接](https://www.luogu.com.cn/problem/P3195)

我们通过观察可以发现，如果我们让 $c_i \leftarrow c_i + 1$，那么得到的效果是让 $L \leftarrow L+1$，这样可以保证题目中的贡献不变且不用考虑填充物的影响。

我们设 $f_{i}$ 表示前 $i$ 个玩具装箱的最小价值，转移就是：

$$
f_i = \min f_j + cost(j + 1, i)
$$

如果 $p_i = \sum_{k = 1}^{i} c_i$ 的话，$cost(i, j)$ 就可以写成 $cost(i, j) = (p_i - p_j - L)^2$。但是直接这么转移是 $O(n^2)$ 的，我们考虑将式子拆开。

$$
\begin{aligned}
f_i &= f_j + (p_i - p_j - L)^2 \\
&= f_j + p_i^2 + (p_j + L)^2 - 2p_i(p_j+L)
\end{aligned}
$$

这里的 $2p_i(p_j+L)$ 是拆不开的，考虑使用斜率优化的一般形式：

$$
Y(i) = K(i)X(i) + C(i)
$$

通过对上式移项可以得到：

$$
\begin{aligned}
f_i &= f_j + p_i^2 + (p_j + L)^2 - 2p_i(p_j + L) \\
f_j + (p_j + L)^2 &= f_i + 2p_i(p_j + L) - p_i^2
\end{aligned}
$$

对比两个式子不难得到 $Y(i) = f_i + (p_i + L)^2, K(i) = 2p_i, X_i=(p_i + L), C(i) = f_i - p_i^2
$，所以可以用单调队列按照这个形式维护下凸壳，每次取队头的最有决策点更新即可。



[code](https://www.luogu.com.cn/record/288734198)



## CF321E. Ciel and Gondolas

[题目链接](https://codeforces.com/problemset/problem/321/E)

给定一个数组 $u_{i, j}$，求将 $n$ 个人分为 $k$ 段，使得 $\displaystyle \sum_{i, j \in S_t} u_{i, j}$ 的最小值。

如果暴力 *DP*，令 $f_{i, j}$ 表示前 $j$ 个人分 $i$ 段得到的最小代价，转移为：

$$
f_{i, j} = \min f_{i - 1, k - 1} + cost(k, j)
$$

其中 $cost(k, j) = \sum_{k \le x \le y \le j} u_{x, y}$，这个可以前缀和预处理出来，但是朴素的转移依旧是 $O(kn^2)$ 的，无法通过。

如果令 $a \le b \le c \le d$，那么 $w(a, c) + w(b, d) \le w(a, d) + w(b, c)$，因为 $w(a, c) + w(b, d) - w(a, d) - w(b, c) = w(a, b - 1) + w(c + 1, d)$，有因为 $u_{i, j} \ge 0$，所以不等式成立。这是一个 **四边形不等式**，所以 *DP* 过程中的最优决策点单调不降，我们就可以通过分治的方式，每次处理处分治中点 $mid$ 的答案，然后递归处理，复杂度 $O(nk \log n)$。

[code](https://codeforces.com/contest/321/submission/384330677)
