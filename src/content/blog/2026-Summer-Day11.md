---
title: '2026 Summer Day11'
description: ''
date: '2026-07-27T14:37:55.142Z'
draft: false
showHeroImage: false
tags: ['DP 优化']
categories: ['记录']
series: ['2026 Summer']
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day11

## P1349 广义斐波那契数列

[题目链接](https://www.luogu.com.cn/problem/P1349)

题目意思就是让你求 $a_i = pa_{i-1} + qa_{i - 2}$ 的第 $n$ 项对 $m$ 取模的结果，但是 $n$ 很大，范围达到了 $2^{31} - 1$，所以线性递推显然过不了。我们考虑仿照斐波那契矩阵加速递推的方式，构造如下矩阵：

$$
\begin{bmatrix}
a_{i-1} & a_{i-2}
\end{bmatrix}
\times
\begin{bmatrix}
p & 1 \\
q & 0
\end{bmatrix}
=
\begin{bmatrix}
a_i & a_{i-1}
\end{bmatrix}
$$

所以我们就可以用矩阵快速幂加速递推了。

[code](https://www.luogu.com.cn/record/288886140)

## CF1152F2. Neko Rules the Catniverse (Large Version)

[题目链接](https://codeforces.com/problemset/problem/1152/F2)

这道题感觉就是神仙题，和出题人脑电波对上了就对了。我们观察发现题目限制都是和值域有关的，所以我们不妨以值域为 _DP_ 状态。我们令 $dp_{i, j, S}$ 表示当前考虑到了第 $i$ 个位置，选择的序列长度为 $j$，且最后 $m$ 个数的选取状态为 $S$，那么显然第 $i$ 个树插入到空位和开头都是合法的。所以我们有如下转移：

$$
\begin{aligned}
dp_{i,j,S} \times (\operatorname{popcount}(S) + 1) &\to dp_{i+1,j+1,(2S+1)\&(2^m-1)} \\
dp_{i,j,S} &\to dp_{i+1,j,(2S)\&(2^m-1)} \\
\end{aligned}
$$

然后发现转移和 $i$ 无关，所以可以直接压掉一维，然后把后两维压入到一个状态里面，变成 $j \times 2^m + S$，就可以使用矩阵快速幂加速递推了。

~~又一道黑题~~

[code](https://codeforces.com/contest/1152/submission/384474367)

## P4719 【模板】动态 DP

[题目链接](https://www.luogu.com.cn/problem/P4719)

这道题如果不考虑修改操作的话，就是 **没有上司的舞会**，我们令 $f_{u, 0/1}$ 表示当前 $u$ 节点选/不选带来的最大价值。则有转移：

$$
\begin{aligned}
f_{u,0} &= \sum_{v \in sun(u)} \max(f_{v, 0}, f_{v, 1}) \\
f_{v,1} &= \sum_{v \in son(u)} f_{v, 0} + w_u
\end{aligned}
$$

但是这里有修改操作，如果每次暴力跳父亲修改的话，复杂度直接爆炸，所以我们需要换种思路。

发现树链剖分是处理树上路径修改问题的有效方式，我们不妨考虑使用树剖来解决这个问题。我们将轻重儿子分开考虑，令 $g_{u, 0/1}$ 表示不考虑 $u$ 的重儿子的最大价值，转移时同上，这样我们就可以将 $f$ 的转移时改写为：

$$
\begin{aligned}
f_{u,0} &= g_{u,0} + \max(f_{\operatorname{wson}(u),0}, f_{\operatorname{wson}(u),1}) \\
f_{u,1} &= g_{u,0} + f_{\operatorname{wson}(u),0} + w_u
\end{aligned}
$$

这样我们最终需要的 $f$ 的表达就简单了许多。然后我们考虑使用矩阵乘法加速。即我们需要一个矩阵 $P$，满足：

$$
\begin{bmatrix} f_{v, 1} & f_{v, 0} \end{bmatrix} \times P = \begin{bmatrix} f_{u, 1} & f_{u, 0} \end{bmatrix}
$$

手推一下不难发现：

$$
P = \begin{bmatrix} g_{v, 0} & g_{u, 1} \\ g_{u, 0} & -\infin \end{bmatrix}
$$

这里的矩阵乘法定义为：

$$
C_{i, j} = \max_{k} A_{i, k} + B_{k, j}
$$

又叫作 Max(+) 卷积。

然后每次修改的时候就可以用矩阵加速重链的递推过程，只需要跨过 $O(\log n)$ 条轻边即可，复杂度 $O(q \log^2 n)$，还有一个 $\log$ 是线段树的。

[code](https://www.luogu.com.cn/record/288982934)
