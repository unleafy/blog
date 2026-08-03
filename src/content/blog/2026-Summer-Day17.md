---
title: "2026 Summer Day17"
description: ""
date: "2026-08-02T07:36:22.524Z"
draft: false
showHeroImage: false
tags: ["博弈论"]
categories: ["记录"]
series: ["2026 Summer"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day17

## QOJ 15321 AGI

[题目链接](https://qoj.ac/problem/15321)

> 将一个序列 $a_i$ 分为两个集合 $A, B$，要求 $\oplus_{v \in A} v = 0$，、问是否可行。 

如果我们统计每个 $x$ 出现的次数 $c(x)$，那么 $x$ 一定是成对被两人轮流取走的，不然先手就更可能达成目标。我们只需要考虑 $\lfloor \frac{c(x)}{2} \rfloor$ 和 $c(x)$ 为奇数的 $x$，令 $num$ 为所有 $\lfloor \frac{c(x)}{2} \rfloor$ 为奇数的 $x$ 的异或和。不难发现先手获胜只有一下两种情况：

+ $c(x)$ 为奇数的 $x$ 的个数为 $0$ 且 $num = 0$;

+ $c(x)$ 为奇数的 $x$ 的个数为 $2$ 且 $num$ 为其中之一。

[code](https://qoj.ac/submission/2707392)

## AGC002 E-Candy Piles

[题目链接](https://atcoder.jp/contests/agc002/tasks/agc002_e)

由于每次要么删除一行，要么删除最多的一列，所以不妨把 $a_i$ 按照猜从大到小的顺序排序，然后问题就转化为了 **在一个梯状地图内，每次可以向右或者向上一步，先碰到地图边界的人失败，求是否先手必胜**。

考虑什么情况下是先手必胜的。假设当前这个点是 $(x, y)$，如果这个点到上边界和右边界的距离有一个为奇数，则当前这个点先手必胜。而且这道题目当中，童宁一条对角线上的胜负状态是一致的。因为每次移动都会走到一条新的对角线上，但是接下来的那个人又可以反着移动到原来的那条对角线上。所以我们考虑 $i$ 最大的、在地图内的点 $(i, i)$ 的胜负状态即可。

[code](https://atcoder.jp/contests/agc002/submissions/78069931)

## ARC168 B-Arbitrary Nim

[题目链接](https://atcoder.jp/contests/arc168/tasks/arc168_b?lang=en)

这道题在原本的 *Nim* 游戏上增加了最多取走 $k$ 个的限制，但是发现 $a_i \bmod (k + 1) \to a_i$ 后，对新的是否必胜的判定就和 *Nim* 游戏一样了。

考虑输出 $-1$ 的情况，若原本的序列满足 $\oplus a_i \ne 0$，那么所有 $k > max\{a_i\}$ 均满足条件。

然后是输出 0 的情况，如果 $a_i = a_j$，则这两个数 $\bmod (k+1)$ 依然是相等的。所以最后是否合法还是和他们出现次数的奇偶性相关。分类讨论即可。

[code](https://atcoder.jp/contests/arc168/submissions/78074836)
