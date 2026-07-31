---
title: "2026 Summer Day14"
description: ""
date: "2026-07-30T11:31:47.115Z"
draft: false
showHeroImage: false
tags: ["DP"]
categories: ["记录"]
series: ["2026 Summer"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day14

## P2150 [NOI2015] 寿司晚宴

[题目链接](https://www.luogu.com.cn/problem/P2150)

考虑 $n \le 30$ 的时候怎么做。由于题目要求 $gcd(x, y) = 1$，即 $x, y$ 互质，所以 $x$ 和 $y$ 里面一定不存在相同的质因数。

那么我们考虑 $x, y$ 中分别有那些质因数，设 $\le n$ 的质数构成的数组为 $p_i$，那么可以直接令 $S, T$ 分别表示 $x, y$ 中包含的质因数，那么一个合法的状态必然满足 $S \operatorname{and} T = 0$。转移即为：

$$
dp_{S, T} = \begin{cases}
1 & S = T = 0 \\
\sum_{k \operatorname{and} S = 0} dp_{S, T \operatorname{or} k} + \sum_{k \operatorname{and} T = 0} dp_{S \operatorname{or} k, T} & otherwise
\end{cases}
$$

但是这个做法在 $n \le 500$ 的时候显得不那么优秀，质数数量明显增多，状态数量指数级上涨，显然是无法接受的。但是考虑和之前模拟赛 Day 1 T3 类似，我们可以考虑拆成 $\le \sqrt{n}$ 的部分和 $> \sqrt{N}$ 的部分，显然一个数只会有至多一个 $> \sqrt{n}$ 的因子。那么我们可以单独考虑这个因子，判断它属于哪一边。

具体而言，我们对于每一个存在的 $> \sqrt{n}$ 的因子，重新用两个新数组 $f_{S, T}, g_{S, T}$ 表示当前这个 $> \sqrt{n}$ 的质因子分别属于两边的方案数，转移同上。最后合并的时候就是

$$
dp_{S, T} = f_{S, T} + g_{S, T} - dp_{S, T}
$$

减去 $dp_{S, T}$ 是因为还多算了一边之前没取这个质因子的贡献。复杂度 $O(n2^16)$。

[code](https://www.luogu.com.cn/record/289638072)

-----

~~好像 B 班就这一道可以写写的~~
不对，还有一道可以写的！

-----

## P10959 月之谜

[题目链接](https://www.luogu.com.cn/problem/P10959)

这道题发现如果直接搜索数字，然后记录数位和并判断整除的话，会特别麻烦，状态也会爆炸。

我们考虑直接枚举最后的数位和是多少，然后每次搜索时碰到数位和等于我们枚举的这个数的状态再记录。

为了避免每次都要清空 *DP* 数组，我们可以至记录 $lim = 0$ 的那些状态的 *DP* 值，对于 $lim = 1$ 的直接暴力计算即可，考虑到这部分数很少，所以直接暴力计算也是对的。

[code](https://www.luogu.com.cn/record/289652296)
