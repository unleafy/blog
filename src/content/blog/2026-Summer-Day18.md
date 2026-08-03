---
title: "2026 Summer Day18"
description: ""
date: "2026-08-03T05:40:11.673Z"
draft: false
showHeroImage: false
tags: ["构造"]
categories: ["记录"]
series: ["2026 Summer"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day18

## CF1110 E - Magic Stones

[题目链接](https://codeforces.com/problemset/problem/1110/E)

> 进行若干次操作，每次可以将 $c_x$ 变为 $c_{x+1} + c_{x-1} - c_{x}$，求是否可以将数组 $c$ b变为数组 $t$。

如果令 $d_i = c_{i+1} - c_{i}$，那么进行一次操作相当于 $d_{i} = c_{i+1} - (c_{i+1} + c_{i-1} - c_{i}) = c_i - c_{i-1}$，$d_{i-1} = (c_{i+1}+c_{i-1}-c_{i})-c_{i-1} = c_{i+1}-c_{i}$，即交换相邻的两个 $d_i$ 的值，所以只需要考虑两个数组的差分数组构成的可重集是否相同即可。

[code](https://codeforces.com/problemset/submission/1110/385351166)

## HDU 6664 - Andy and Maze

[题目链接](https://acm.hdu.edu.cn/showproblem.php?pid=6664)

> 给一张无向带权图，求走过 **恰好 $k$ 个点的简单路径的边权之和的最大值**。
> 
> $n, m \le 10^4$

考虑随机化做法。给每个点随机一个 $[0, k)$ 之间的随机颜色，然后要求求出一条路径使得路径上 $[0, k)$ 之间的每种颜色恰好出现一次。然后直接枚举当前的颜色状态和转移的点即可。单次解答的时间复杂度为 $O(2^knm \times P)$，其中 $P$ 是随机赋值的次数。

[code](https://acm.hdu.edu.cn/viewcode.php?rid=40946344)

## QOJ 141 - 8染色

[题目链接](https://qoj.ac/problem/141)

> 给你一张无向图 $G(u, v)$，并给你一个合法的 8-染色 方案，你需要构造一种方式，使得另一个程序可以通过这个构造返回一个这张图的合法 8-染色方案，传输的构造为一个 0/1 数组，不超过 2.5e5 个 bit.

很显然直接将原来的合法构造传过去是一个无脑的方案。

考虑如何优化。不难发现，如果一个点满足 $\deg(u) < 8$，那么这个点显然可以通过遍历它的邻接点来找到一个尚未被使用的颜色，所以可以直接不传 $\deg(u) < 8$ 的点的方案。这时候的数据大小是 $\frac{3m}{4}$ 的，仍然无法通过。

如果我们考虑将表示颜色的三个 bit 转换为两个 bit，这个时候我们可以通过枚举最后一个 bit 的取值来找到这个点的颜色。所以传输大小缩小至 $\frac{m}{2}$，可以通过。 

[code](https://qoj.ac/submission/2714343)

## CF1887 E - Good Colorings

[题目链接](https://codeforces.com/problemset/problem/1887/E)

考虑将原网格图转化为二分图的形式，则这张二分图中必然存在偶环，所以我们提取一个偶环出来，然后每次将它对半劈开，询问劈开这条边的颜色，然后找到不存在这个颜色的哪一半环作为下一个偶环，重复这个操作。由于每次环长减小一半，所以总询问

[code](https://codeforces.com/problemset/submission/1887/385402985)
